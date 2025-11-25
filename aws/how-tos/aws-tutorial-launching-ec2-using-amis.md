# AWS Tutorial: Launching EC2 Instances and Using AMIs
**Objectives:**
- Successfully launch an EC2 instance
- Launch an instance using a custom AMI (Amazon Machine Image)
- Connect to and manage your instance
- Understand difference between using a base image and custom image

### Prerequisites
- [ ] AWS Console access
- [ ] SSH Client
- [ ] Access to AMIs in permissions

## Part 1: Understanding AMIs vs. Base Images
**Base Images** are basic operating systems (like Ubuntu or Amazon Linux) that
don't come with anything installed. It's like buying a blank computer and then
needing to install all your software yourself. Everytime you launch one you start
from scratch and might spend hours installing Python, R, and other bioinformatics tools needed.

**AMIs (Amazon Machine Images)** are like using computers where everything has
been pre-installed. Someone has to have already installed all the tools needed and
saved the environment as a template to be launched.

|  | Base AWS Image | Custom AMI |
|--------|---------------|------------|
| **Purpose** | Generic starting point | Pre-configured for specific tasks |
| **Software** | Basic OS only | Your tools pre-installed |
| **Use Case** | Learning, custom setups, creating AMIs | Production work, team consistency |

## Part 2: Launching a Basic Instance (Base Image)
2.1 Log into AWS
1. Open your web browser and go to the AWS Console
2. Sign in using MFA and your appropriate credentials
3. **Verify your region** in the top-right corner (should be Oregon us-west-2)

2.2 Navigate to EC2 & Launch Instance
1. In the search bar, type "EC2" and select "EC2"
2. You should see the EC2 Dashboard
3. Click "Launch Instance"

2.3 Choose Base Amazon Machine Image
1. You'll see the "Launch an Instance" page
2. Give your instance a name under "Name and tags"
3. Under "Application and OS Images (Amazon Machine Image)" you'll see some tabs such as **Quick Start**.
    - Common base images to select:
      - **Amazon Linux 2 AMI** - AWS' optimized Linux
      - **Ubuntu Server 24.04 (or 22.04) LTS** - common for bioinformatics work
      - **Windows server** - for Windows based workflows
4. Typically we select `Ubuntu Server 24.04 LTS, SSD Volume Type`

2.4 Choose Instance Type
1. For learning/testing choose something free tier eligible: `t3.micro`, `t3.small`
2. For larger work you can choose something based on your CPU/Memory needs. I would
review the pricing of whatever instance you decide on choosing [here](https://aws.amazon.com/ec2/pricing/on-demand/) to get an idea of costs.

2.5 Key Pair (login)
1. If you don't have a key pair:
  - Select "Create a new key pair"
  - **Key pair name**: `first_last_key_pair`
  - **Key pair type**: `RSA`
  - **Private key file format**: `.pem`
  - **Create your key pair** and **save the file securely** -- you cannot download it again
2. If you have a key pair then select your key pair.
3. (Optional) You can save your key pair somewhere like to keep things organized `~/.ssh/my-key.pem`
  - If you're interested in doing that you'd need to:
    ```
    # Create SSH directory if it doesn't exist
    mkdir -p ~/.ssh
    # Move your downloaded key there
    mv ~/Downloads/my-key.pem ~/.ssh/
    # Set proper permissions on the key file
    chmod 400 ~/.ssh/my-key.pem
    ```


2.6 Network Settings
1. You can keep the network, subnet, auto-assign public IP, and firewall settings at default. Default would look like:
  - **Network**: default
  - **Subnet**: default
  - **Auto-assign public IP**: Enabled
  - **Firewall**: create security group
2. For your security group rules make sure you:
  - **Allow SSH traffic from**: My IP (this restricts access to your current IP address)

2.7 Configure Storage
1. **Root volume**: 8 GB (default) but you can increase this depending on your work needs
2. **Volume Type**: General Purpose SSD (gp3)
3. **Delete on Termination**: Yes (this is usually the default and you can see this if you go to Advanced and expand the details of the storage)

2.8 Launch your Instance
1. Launch your instance, you can see a summary of your instance on the right hand panel usually
2. To see your Instance state go to EC2 > Instances. The name of your instance and information on its state should be listed.

## Part 3: Connecting to your Basic Instance
3.1 Connecting to your instance
1. Select your instance in the EC2 Console
2. **Verify your instance is Running** and status checks show 2/2checks passed
3. Click "Connect" button
4. Click "SSH Client" tab
3.2. Prepare your key file if you haven't already
Before connecting, ensure your key file has the correct permissions:
`chmod 400 "my_key_pair.pem"`
3.3 Connect via SSH
1. Approach 1: From the key file directory
`ssh -i "my-first-key.pem" ubuntu@ec2-xx-xx-xx-xx.compute-1.amazonaws.com`
2. Approach 2: Using the full path to key
`ssh -i "~/.ssh/my-key.pem" ubuntu@ec2-xx-xx-xx-xx.compute-1.amazonaws.com`

And you should be all connected to your basic instance! Navigate around and see what's there and what needs to be installed.

## Part 4: Terminating your Instance
4.1 Save Important Data
1. Before terminating your instance save important data you need in your S3 bucket. Once terminated all data on the instance will be lost. For how to interact with S3 see [tutorial here](https://github.com/NW-PaGe/mep-cloud-resources/blob/main/aws/how-tos/aws-tutorial-upload-download-to-s3.md).

4.2 Terminate Instance
1. Go to EC2 Dashboard
2. Select "Instances" in the left menu
3. Select your instance (check the box)
4. Click "Instance State" dropdown at the top
5. Select "Terminate Instance"
6. Confirm by clicking "Terminate"

4.3 What Happens After Termination

1. Instance state changes to "Shutting-down" then "Terminated"
2. All data on the instance is permanently lost
3. EBS root volume is deleted (if "Delete on termination" was enabled)
4. Billing stops immediately
5. Instance ID becomes invalid - cannot be reused

4.4 Alternate: Stop Instead of Terminate

If you need the instance and the contents on it again for a short-term analysis you can choose to "Stop Instance" in the "Instance State" dropdown.
You'll see that the instance will be listed as "Stopped" in its status on your EC2 > Instance Dashboard

4.5 Verify that your instance has been Terminated or Stopped
- Both instances should show "Terminated" or "Stopped" state
- No instances should show "Running"
- Your billing will stop for these instances

4.6 Instances States and their meaning
- **Pending**: Starting up
- **Running**: Active and billable
- **Stopping**: Shutting down
- **Stopped**: Shut down, not billable (but EBS storage still costs)
- **Terminating**: Being deleted
- **Terminated**: Permanently deleted

## Part 5: Launching with a Custom AMI
Now let's launch an instance with a pre-configured AMI

5.1 Find your Custom AMIs
1. Go back to EC2 Dashboard > Launch Instance
2. Under "Applications and OS Images" go to the My AMIs tab instead of Quick Start.

5.2 Select the AMI you want to use

5.3 Configure the AMI Instance as mentioned in steps 1-4 above

5.4 Launch AMI-based Instance

5.5 Explore the AMI-based Instance to confirm that the appropriate software are installed.

## Part 6: Creating your own AMI
Once you've customized a basic instance to your software and database requirements you can save it easily as your AMI.
6.1 Customize your basic instance
- Install whatever needed database files and software onto your basic instance
6.2 Create AMI from your instance
1. Through AWS CLI
```
aws ec2 create-image \
    --instance-id i-1234567890abcdef0 \
    --name "My-Custom-Bio-AMI-$(date +%Y%m%d)" \
    --description "Custom bioinformatics AMI with basic tools" \
    --no-reboot
```
2. From AWS Console
  1. Select your instance in EC2 > Instance Dashboard
  2. Actions > Image and templates > Create image
  3. Image Name: name your image. include relevant software
  4. Reboot instance: you can leave it clicked but it will reboot your instance and
  kick you off your SSH connection to it briefly. The benefits of rebooting are
  you're ensuring maximum data consistency and that everything on the instance is being written to disk properly.
  5. Click "Create image"

6.3 Checking your AMI creation

It takes time for the AMI to be created but you can check on its creation if you navigate to EC2 > Images (on the left hand panel) > AMIs.
You'll see the status of the AMI and it will be ready for use once the status is "Available"
