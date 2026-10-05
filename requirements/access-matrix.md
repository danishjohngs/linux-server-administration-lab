# Access Matrix

## Users

| User | Role | Groups | Required Areas |
|---|---|---|---|
| Alice |Developer |developers |/app, /shared |
| Bob |Developer |developers |/app, /shared |
| Charlie |Operations Engineer |operations, deployers |/app, /shared, /deploy, /logs |
| David |Security Analyst |security |/logs, /security |
| deploy |deployment account |deployers |/deploy |

## Directory Requirements

| Directory | Required Users/Groups | Purpose | Required Access |
|---|---|---|---|
| /company/app |Alice,Bob,Charlie |Application development and management|Alice/Bob: rwx • Charlie: rwx |
| /company/shared |developers, Charlie |Shared development area |Alice/Bob: rwx • Charlie: rwx |
| /company/deploy |deploy, Charlie |Deployment files |deploy: rwx • Charlie: rwx |
| /company/logs |Charlie,David |Application logs and auditing |Charlie: r-x • David: r-x |
| /company/security |David |Security information |David: rwx |