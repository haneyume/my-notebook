# supabase-pgvector

## References

- [Supabase Blog](https://supabase.com/blog/openai-embeddings-postgres-vector)

## Steps

```sql
CREATE EXTENSION pgvector;
```

```sql
create table documents (
  id bigserial primary key,
  content text,
  embedding vector (1536)
);
```

```sql
create or replace function match_documents (
  query_embedding vector(1536),
  similarity_threshold float,
  match_count int
)
returns table (
  id bigint,
  content text,
  similarity float
)
language plpgsql
as $$
begin
  return query
  select
    id,
    content,
    1 - (documents.embedding <=> query_embedding) as similarity
  from documents
  where 1 - (documents.embedding <=> query_embedding) > similarity_threshold
  order by documents.embedding <=> query_embedding
  limit match_count;
end;
$$;
```

| Operator | Description            |
| -------- | ---------------------- |
| <->      | Euclidean distance     |
| <#>      | negative inner product |
| <=>      | cosine distance        |

### Indexing

```sql
create index on documents
using ivfflat (embedding vector_cosine_ops)
with (lists = 100);
```
