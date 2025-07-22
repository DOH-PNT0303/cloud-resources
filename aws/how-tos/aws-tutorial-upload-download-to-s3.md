# AWS CLI: How to transfer files to S3 buckets

### Step 1: Connect to your EC2 instance
If you connect using Google Remote Desktop skip to Step 2.

1. Open your terminal.
2. Connect to your EC instance using SSH:
```
ssh -i /path/to/your/key.pem ec2-user@<ec2-public-ip>
```

### Step 2: Check if AWS CLI is installed through Terminal
Run:
```
aws --version
```
- If you see output like: aws-cli/2.13.30 Python/3.x, then you're good.
- If you see "command not found" install it using

For ubuntu:
```
sudo apt update
sudo apt install awscli -y
```

For Amazon Linux:
```
sudo yum install aws-cli -y
```

### Step 3: Configure AWS CLI (One-Time Setup)
1. Check that AWS CLI is configured
```
aws configure list
```

Example Output:
```
Name                    Value             Type    Location
----                    -----             ----    --------
profile                <not set>             None    None
access_key     ****************ABCD shared-credentials-file
secret_key     ****************1234 shared-credentials-file
region                us-west-2      config-file    ~/.aws/config
```
If access_key, secret_key, and region show up then you should be configured!

You can also test with:
```
aws s3 ls
```

Example Output First Lines:
```
2024-12-04 10:43:12 aws-glue-assets-389563606246-us-west-2
2024-11-12 00:10:07 aws-glue-gisaid
...
```
You should see all the buckets available within the MEP team AWS account.

2. <b>If AWS CLI is not configured</b>, you will need to make sure you have your AWS Access keys. This should be in an csv with the fields AWS Access Key ID and AWS Secret Access Key. <i> If you have lost that file let your account Admin know and you will be issued a new set of credentials. </i>  

Once you have those handy run:
```
aws configure
```

It will ask you for:

<b> AWS Access Key ID </b>: copy and paste your key ID here
<b> AWS Secret Key ID </b>: copy and paste your key ID here
<b> Default region </b>: us-west-2
<b> Default output format </b>: Press Enter for default (json)

3. Follow previous steps to check that you can see the S3 buckets

### Step 4: Upload Files to S3
Now you're ready to transfer files back and forth from your machine to the S3 buckets. The main commands you want to use are `aws cp` (to copy files individually or recursively move contents of a directory) and `aws sync` (to sync the contents of the origin directory with the destination). AWS CLI is structured where you use a command such as `aws cp <origin> <destination>` to specify where the origin file/folder is and where you want the destination file/folder to go.

- Upload a single file to S3
```
aws s3 cp myfile.txt s3://your-bucket-name/subfolder-if-applicable
```
- Upload a folder (recursively) with all the contents in the folder
```
aws s3 cp myfolder/ s3://your-bucket-name/subfolder-if-applicable --recursive
```
- Sync a local directory's entire contents with an S3 folder
```
aws s3 sync myfolder/ s3://your-bucket-name/subfolder-if-applicable/
```

### Step 5: Verify Upload
You can list what's in your bucket or folder to check that the file is there
```
aws s3 ls s3://your-bucket-name/
aws s3 ls s3://your-bucket-name/subfolder-if-applicable/
```

### (Optional) Pulling files from S3
Similarly to pushing files to S3 you can use `aws cp` and `aws sync` to retrieve files from S3 to your local machine.

- Download a single file from S3
```
aws s3 cp s3://your-bucket-name/subfolder/myfile.txt .
```
- Sync an S3 directory with a local directory
```
aws s3 sync s3://your-bucket-name/subfolder/ mylocalfolder/
```
