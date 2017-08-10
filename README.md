# AWS Tools

Some scripts to assist in using multiple aws accounts, using mfa and environment variables for access.
# awstoken
This is only needed for MFA & delegated access accounts. Non MFA /or root accounts do not require this.
WARNING: This will override your config and credentials files and manage them itself.

Requires:
* A master credentials file
* `AWS_HOME` Bash environment variable set to the aws home where the credentials live (e.g ~/.aws)

**Master Credentials File**
Should be located at `AWS_HOME/master_credentials.yaml`
There are 3 possible types of entries
1) Root credentials account (or IAM Account no/MFA)
2) IAM Account with MFA
3) Role based account that is delegated from another account

FORMAT (with all possible parameters):
```
<Title>:
    aws_access_key_id: <key>
    aws_secret_access_key: <secret>
    region: <region>
    output: <format>
    root: <true>
    iam_number: <account-number>
    iam_user: <iam-username>
    iam_role: <iam-delegated-role>
    parent_account: <Title-of-parent-account>
```
----
PARAMETERS:

To select, you must always provide all 4 below, plus the parameters from one of the options below.

`aws_access_key_id`: (Required All Options)
`aws_secret_access_key`: (Required All Options)
`region`: (Required All Options)
`output`: (Required All Options)
**Option 1)**
`root`: (Required)
Example:
```
my_root_account:
  aws_access_key_id: <key>
  aws_secret_access_key: <secret>
  region: us-west-2
  output: json
  root: true
```
**Option 2)**
`iam_number`: (Required)
`iam_user`: (Required)
Example:
```
my_auth_account:
  aws_access_key_id: <key>
  aws_secret_access_key: <secret>
  region: us-west-2
  output: json
  iam_number: '2837479202134'
  iam_user: 'username'
```
**Option 3)**
`iam_role`: (Required)
`iam_number`: (Required)
`parent_account`: (Required)
Example:
```
my_delegated_account:
    parent_account: 'my_auth_account'
    region: us-west-2
    output: json
    iam_number: '2356245634456'
    iam_role: 'devops'
```
**Usage**

Running `awstoken` without any options will return the syntax and a list of available profiles.
```
Missing profile name
Syntax $HOME/bin/awstoken <profile name> <token code>
Available Profiles:
 - my_root_account (Root Keys - No Exp / No MFA)
 - my_auth_account (Expired!)
 - my_auth_account2 (Expired!)
```
A successfull run will output nothing and just return.

# awsenv
This must be setup as a bash function vs as a bin type executable as it needs to be able to manipulate the environment variables in your active environment.

For example:
* Assuming you put the file somehwere like `$HOME/bin/functions/awsenv`
* Source it in a bashrc or similar as `source $HOME/bin/functions/awsenv`

Usage:
Running it without arguments will yield the usage help.
```
Missing profile name
Usage: awsenv <profile name>
```
It is designed to match the profile names in the `$AWS_HOME/credentials` file.
For example:
```
$> awsenv myprofile
Success (Profile: myprofile)
$>
```

