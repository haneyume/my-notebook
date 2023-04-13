# Triggers

## Overview

| Trigger           | Condition        | Action                                          |
| ----------------- | ---------------- | ----------------------------------------------- |
| following_created |                  | increase followingCount / followerCount of User |
| following_deleted |                  | decrease followingCount / followerCount of User |
| post_created      | parentId == null | increase postCount of User                      |
| post_deleted      | parentId == null | decrease postCount of User                      |
| post_created      | parentId != null | increase commentCount of Post                   |
| post_deleted      | parentId != null | decrease commentCount of Post                   |
| post_like_created |                  | increase likeCount of Post                      |
| post_like_deleted |                  | decrease likeCount of Post                      |
