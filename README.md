# aws-utils
Utilities for working with AWS resources.

## start-ec2 / stop-ec2

These simple utilities are designed to facilitate stopping and starting of AWS EC2 instances, and work explicitly to work with an suporrt the configuration options for SSH access. The three supported SSH options are: 
1. Basic SSH configuration. The main problem here is that IP addresses for EC2 instances on AWS change whenever they are restarted, so that accessing them requires manually acquiring and changing the IP address in the SSH config. However, because an Instance ID does not change (unless terminated), it's possible to start an instance, harvest the new IP address, and swap it out with the old one in the SSH config file. 
2. EC2 Instance Connect Endpoints. With an endpoint, you do not use the instance IP address explicitly in the SSH config, so these utilities simply parse the config to harvest the Instance ID, SSO profile, and region to start the instance. 
3. Private IP access. WIth this configuration, users SSH into the EC2 instance with it's private IP address, which does not change when restarting an instance.

UPDATE: The code has been modified temporarily to work witih AWS SSO accounts. If using SSO you must be logged in under the desired profile.

### Installation 

Prerequisites: 

i. A properly configured [AWS CLI](https://aws.amazon.com/cli/) utility.  
ii. A properly configured `~/.ssh/config` file.  
iii. A bash shell. 

Then do:  

1. Clone this repository to `~/.aws/`. 
2. Change permissions to make executable:
```
sudo chmod 755 <path to home folder>/.aws/aws-utils/start-ec2
sudo chmod 755 <path to home folder>/.aws/aws-utils/stop-ec2
```
3. Symlink each script somewhere in your PATH. For example: 
```
sudo ln -s <absolute path to home folder>/.aws/aws-utils/start-ec2 /usr/local/bin/start-ec2
sudo ln -s <absolute path to home folder>/.aws/aws-utils/stop-ec2 /usr/local/bin/stop-ec2
```
4. Modify the scrips to contain the appropriate defaults for Host and Instance Type, if desired.  
5. If using the basic SSH configuration, create a file `~/.aws/ec2-ssh.json` that maps Instance IDs to SSH Hosts in your `.ssh` folder. Follow [the template provided](https://github.com/tfarkas86/aws-utils/blob/main/ec2-ssh.json). 
### Usage
`start-ec2` takes three arguments:  
  -h: Host. Not required if default is set in script. From your `~/.ssh/config`.  
  -p: Profile: Optional here, but a profile name is required either in the config json, environment variable AWS_DEFAULT_PROFILE, or in this argument. Not required if using an Instance Connect Endpoint.
  -t: Instance type. Not required, but will change your instance type if you like. See AWS EC2 documentation. E.g., 'r5.xlarge'.
  
 `stop-ec2` takes the `-h` and `-p` arguments.
 
Once the instance is started, you will received a message to stdout indicating the new IP address, and your `~/.ssh/config` file will be accordingly updated. Use the `ssh` command, e.g., `ssh my-ec2-name` to ssh into the running instance. 
