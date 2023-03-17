# aws-utils
Utilities for working with AWS resources.

## start-ec2 / stop-ec2

These simple utilities are designed to solve the basic problem that IP addresses for EC2 instances on AWS change whenever they are restarted, so that accessing them may require a bit of fuss. However, because an Instance ID does not change (unless terminated), it's possible to start an instance, harvest the new IP address, and swap it out with the old one wherever it's needed. My preferred approach to accessing EC2 is by leveraging the ssh config file, which the start-ec2 utility automatically updates, facilitating access after the instance is restarted. 

To use this utility, you must create a file at `~/.aws/ec2-ssh.json` that links EC2 Instance IDs to Hosts in `~/.ssh/config/`. See the example included in this repository. 

`start-ec2` takes two arguments:
  -h: Host, from ~/.ssh/config
  -t: Instance type, see AWS EC2 documentation. E.g., 'r5.xlarge'
  
 `stop-ec2` is in developement. Modify the source code to target an EC2 instance by ID. 
