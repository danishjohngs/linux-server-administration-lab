# Linux Server Administration & Troubleshooting Lab

A hands-on Linux administration project simulating the responsibilities of a Junior Linux / DevOps Engineer managing access and troubleshooting issues in a company's Linux environment.

## Project Overview

**Company:** NexaCloud  
**Role:** Junior Linux / DevOps Engineer  
**Environment:** Ubuntu on WSL2

The objective of this project is to apply Linux administration concepts through practical configuration, access-control testing, and incident investigation.

## Objectives

- Manage Linux users and groups.
- Understand primary and supplementary group membership.
- Create and organize a company directory structure.
- Configure file ownership and permissions.
- Implement and inspect POSIX Access Control Lists (ACLs).
- Test authorized and unauthorized access.
- Investigate Linux permission-related incidents.
- Apply targeted fixes and verify the results.
- Document troubleshooting findings and lessons learned.

## Technology & Tools

- **Operating System:** Ubuntu Linux
- **Environment:** Windows Subsystem for Linux (WSL2)
- **Editor:** Visual Studio Code
- **Version Control:** Git
- **Repository Hosting:** GitHub
- **Access Control:** Linux permissions and POSIX ACLs

## Simulated Company Environment

The project uses a fictional company named NexaCloud, with users representing different responsibilities:

| User | Role |
|---|---|
| Alice | Developer |
| Bob | Developer |
| Charlie | Operations Engineer |
| David | Security Analyst |
| deploy | Deployment account |

The directory structure represents different company resources:

```text
/company/
├── app/
├── shared/
├── deploy/
├── logs/
└── security/
```

Access is configured according to the responsibilities of each user and group.

## Completed Work

### 1. User & Group Administration

- Created users and groups for different company roles.
- Configured primary and supplementary group memberships.
- Investigated and corrected an incorrect group assignment.
- Verified user identities and memberships.

### 2. Filesystem Administration

- Created the company directory structure.
- Organized application, shared, deployment, logs, and security directories.
- Verified directory ownership and structure.

### 3. Permissions & ACLs

- Configured directory ownership and standard Linux permissions.
- Applied user-specific ACL entries.
- Tested file creation, reading, modification, and deletion.
- Verified that unauthorized users could not access restricted resources.

### 4. Incident Investigation & Troubleshooting

#### Incident 01 — Permission Denied

**Problem:** Alice could access and read the shared development directory but could not create or delete files. Bob could work normally.

**Investigation:** Compared user behavior and inspected the directory's ACL configuration.

**Root cause:** Alice's ACL entry lacked write permission.

**Resolution:** Corrected Alice's ACL entry and verified that the required operations worked.

#### Incident 02 — Log Access Failure

**Problem:** Charlie could access the logs directory but could not read `app.log`. David could read the file.

**Investigation:** Examined file ownership, permissions, group membership, and ACL configuration.

**Root cause:** The log file's original ownership and permissions did not grant Charlie read access.

**Resolution:** Changed the file's group to `operations` and configured permissions so that David retained read/write access, Operations group members received read access, and other users had no file access.

**Verification:** Tested the access requirements for Charlie and David and checked that Alice, Bob, and `deploy` could not read the log.

## Troubleshooting Methodology

Each incident follows a structured process:

1. Understand the reported problem.
2. Reproduce the issue.
3. Compare the behavior of affected and unaffected users.
4. Inspect permissions, ownership, groups, and ACLs.
5. Identify the root cause.
6. Apply a targeted fix.
7. Verify the fix and check for unintended access.
8. Document the incident and lessons learned.

## Repository Structure

```text
linux-server-administration-lab/
├── README.md
├── requirements/
│   └── access-matrix.md
├── documentation/
├── incidents/
│   ├── incident-01-permission-denied.md
│   └── incident-02-log-access-failure.md
└── screenshots/
```

The `screenshots/` directory contains evidence from environment setup, user and group administration, filesystem configuration, access-control testing, and incident investigation.

## Key Learnings

- Linux permissions control access to files and directories.
- Primary and supplementary groups affect how users receive access.
- ACLs provide more granular access control than basic permission bits alone.
- Directory permissions behave differently from regular-file permissions.
- Changing ownership or permissions should be based on the access requirements.
- Troubleshooting requires comparing actual behavior with the intended configuration.
- Verification is necessary after every configuration change.

## Project Status

**Completed:** Requirements, environment setup, user and group administration, filesystem setup, access-control configuration, and two permission-related troubleshooting incidents.

**Scope:** This is an introductory hands-on project. Further Linux administration topics and more advanced troubleshooting scenarios will be explored in subsequent projects.

## Author

A hands-on Linux and DevOps learning project focused on practical administration, troubleshooting, access control, and documenting engineering work.
