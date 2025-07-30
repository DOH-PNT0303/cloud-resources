## Admin Setting up new users on MEP

This is a how-to step by step guide for AWS Admin on creating accounts for new users in the MEP AWS resources.

### 1. Create S3 buckets for each user
Create S3 bucket for each user to use as their cloud workspace. Follow the naming convention wamep-firstname-lastname-bucket.

### 2. Create IAM Users with Console Access
Create IAM Users following FirstName_LastName convention. Download console access credentials and send to user.

### 3. Generate access keys for each IAM user.
Go to the IAM User and generate access keys for CLI. Download and send to the new user.

### 4. Generate individual policy and attach to respective user
Individual policies grant a user full access to their personal buckets and inability to view/edit inside others' personal buckets. Create a individual policies that mirror current members' permissions e.g. onboarding-user-permissions

Attach that policy to the IAM User.

### 5. Add users to shared groups
Adding users to shared groups grants shared group-level permissions such as editor access to shared buckets. Add new users to Molecular Epidemiologists and Nextstrain Jobs to give access to shared MEP buckets and ability to use AWS Batch with Nextstrain.
