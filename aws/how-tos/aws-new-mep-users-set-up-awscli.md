# Set up AWS CLI for new MEP users
## AWS CLI access
1. Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
2. Once AWSCLI is installed configure your default profile using `aws configure`
3. Follow prompts for the default profile being created at `~/.aws/credentials`.

It will ask you for:

- <b> AWS Access Key ID </b>: copy and paste your key ID here
- <b> AWS Secret Key ID </b>: copy and paste your key ID here
- <b> Default region </b>: us-west-2
- <b> Default output format </b>: Press Enter for default (json)

Your profile will eventually look something like this within `~/.aws/credentials`:

```
[default]
aws_access_key_id = A***********************
aws_secret_access_key = S****************************************
```
4. AWS region: us-west-2
5. Press Enter for default output format json.
6. To view the profile that you just created you can `vi` or `nano` the credentials file at `~/.aws/credentials`
