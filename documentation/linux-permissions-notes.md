# Linux Permissions — Project Notes

## 1. Standard Linux Permissions

Linux permissions determine which users can read, modify, or execute files and directories.

| Permission | File | Directory |
|---|---|---|
| `r` — Read | Read file contents | List directory entries |
| `w` — Write | Modify file contents | Create, delete, or rename entries, subject to other permission checks |
| `x` — Execute | Execute a program or script | Enter or traverse the directory |

## 2. Permission Categories

Linux permissions are normally represented for three categories:

- **Owner:** The file's owning user.
- **Group:** The file's owning group.
- **Others:** Everyone who does not match the owner or owning group, subject to ACL rules.

Example:

`-rw-r-----`

- Owner: read and write
- Group: read only
- Others: no permissions

## 3. Users and Groups

Groups help administrators manage access according to job responsibilities.

This project used groups such as:

- `developers`
- `operations`
- `security`
- `deployers`

A user can have a primary group and additional supplementary groups.

## 4. Ownership

A Linux file has one owning user and one owning group.

Ownership can be inspected with `ls -l`. Administrators can change ownership or group ownership when justified by the access requirements.

Changing ownership is not always necessary; standard permissions or ACLs may be more appropriate for granting additional access.

## 5. POSIX Access Control Lists (ACLs)

ACLs allow more specific permissions for individual users and groups beyond the basic owner/group/others entries.

In this project, ACLs were used to investigate and correct a user-specific permission problem.

The `getfacl` utility displays ACL entries. A `+` after the permission string in `ls -l` commonly indicates that additional ACL entries exist.

## 6. Troubleshooting Principles

When investigating an access problem:

1. Reproduce the issue using the affected account.
2. Compare the behavior with an account that works.
3. Inspect directory and file permissions.
4. Check ownership and group membership.
5. Inspect ACL entries when applicable.
6. Apply the smallest appropriate change.
7. Verify the required access and confirm unrelated users did not gain access.

## 7. Lessons From This Project

- Directory permissions and file permissions control different operations.
- A user may be able to read a file but not create or delete directory entries.
- Group membership can grant access to multiple users with the same role.
- ACLs are useful for specific access requirements.
- Successful troubleshooting requires verification after the fix.
