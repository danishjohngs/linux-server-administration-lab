# Incident 02 — Log Access Failure

## Incident

Charlie reported that he could access `/company/logs`, but he could not read the `app.log` file.

David from the Security team could read the same log file without any problem.

## Impact

Charlie was affected.

He could:

- Enter `/company/logs`
- List the files in the directory

He could not:

- Read `app.log`
- Modify `app.log`

David was not affected and could access the log normally.

## Investigation

I first reproduced the problem as Charlie.

Charlie could enter the logs directory and see `app.log`, but reading the file failed.

I then checked the file permissions:

```bash
ls -l /company/logs

The file showed:
-rw------- 1 david security 99 ... app.log

This meant:
- David, the owner, had read and write access.
- The security group had no access.
- Other users had no access.
I checked Charlie's group membership and found that Charlie belonged to the operations group, not the security group.
I also checked the ACL:
getfacl /company/logs/app.log

There was no additional ACL entry granting Charlie access.
Root Cause
The app.log file was owned by:
david:security

with permissions:
-rw-------

Charlie was not the owner and was not a member of the security group.
Therefore, Charlie had no permission to read the file.
David could read the file because he was the owner.
Resolution
Since Charlie is an Operations Engineer and operations users require access to application logs, I changed the file's group from security to operations:
sudo chown david:operations /company/logs/app.log

I then configured the file permissions so that:
- David can read and write.
- Members of operations can read.
- Everyone else has no access.
sudo chmod 640 /company/logs/app.log

The final permissions were:
user::rw-
group::r--
other::---

Verification
I verified the final access behavior.
Charlie
- Read app.log: ✅
- Modify app.log: ❌
David
- Read app.log: ✅
- Modify app.log: ✅
Unrelated Users
- Alice → cannot read app.log: ✅
- Bob → cannot read app.log: ✅
- deploy → cannot read app.log: ✅
No unintended access was introduced.
Lesson Learned
This incident demonstrated how Linux file ownership, group membership, and standard permissions work together.
A file can have only one owner, but a group can provide access to multiple users with the same role.
The troubleshooting process was:
Report → Reproduce → Compare Users → Inspect Ownership & Permissions → Identify Root Cause → Fix → Verify
It also reinforced the principle:
Use groups when access follows a role; use ACLs when a specific user needs an additional or exceptional permission.