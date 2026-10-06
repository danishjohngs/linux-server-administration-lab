# Incident 01 — Permission Denied

## Incident

Alice reported that she could access `/company/shared` and read files, but she could not create or delete files in the shared development area.

Bob reported no issues and could continue working normally.

## Impact

Alice was affected.

She could:

- Enter `/company/shared`
- List files
- Read files
- Modify existing files

She could not:

- Create new files
- Delete files

Bob was not affected and had full required access.

## Investigation

I first tested the actual behavior as Alice and Bob.

### Alice

- Enter directory: ✅
- List contents: ✅
- Create file: ❌
- Modify existing file: ✅
- Delete file: ❌

### Bob

- Enter directory: ✅
- Create file: ✅
- Modify file: ✅
- Delete file: ✅

I then inspected the ACL configuration:

```bash
getfacl /company/shared

The relevant ACL entry was:
user:alice:r-x

Alice had read and execute permissions but did not have write permission on the directory.
Root Cause
The ACL for Alice on /company/shared was incorrectly configured as:
user:alice:r-x

For a directory, write permission is required for operations such as creating and deleting files.
Therefore, Alice could enter and read the directory but could not create or delete files.
Bob continued to work because he received full access through the developers group:
group::rwx

Resolution
I corrected Alice's ACL entry:
sudo setfacl -m u:alice:rwx /company/shared

The ACL now contains:
user:alice:rwx

Verification
After applying the fix, I verified that Alice could:
- Modify an existing file ✅
- Delete a file ✅
Bob was also tested again and could:
- Create a file ✅
- Modify a file ✅
- Delete a file ✅
No unrelated users were given access because the change targeted only Alice's ACL entry.
Lesson Learned
This incident demonstrated that Linux access cannot always be determined from the basic permission bits alone.
ACLs can provide user-specific permissions that change a user's effective access.
The troubleshooting process was:
Report → Test behavior → Compare users → Inspect ACL → Identify root cause → Fix → Verify
```