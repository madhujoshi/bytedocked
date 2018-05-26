# Installation Instructions
Follow these steps to setup the ByteCubed DevOps Challenge node.  The instructions are designed to meet these requirements.

 - Use a configuration management tool of your choice (i.e. Ansible,
   Chef, Puppet, etc.) to configure a Linux server as a docker host.
 - Build a container with a webserver
 - Configure https (self-signed certificate is acceptable)
 - Configure http redirect to https
 - Configure the http/https homepage to display “BYTECUBED DEVOPS
   CHALLENGE”
 - Deploy the container on the Docker host.
 - Configure host-based firewall for ports 22, 80 and 443
 - Forward ports 80 and 443 to the docker web server container.
 - Create an automated test to verify the web server is listening on
   port 443

# Tools used

 - AWS EC2 instance
 - Ansible 2.5.3 on Mac OS X
 - Docker
 - nginx
 - Github public repo for version control

# Source code
The source code (Ansible playbooks) can be found at **[ByteDocked Github Repo](https://github.com/madhujoshi/bytedocked)**

## Assumptions and setup


 - Create or choose AWS keypair to use for the instance. The example
   assumes **bytecubed-test-keypair**.  You can override the keypair by
   passing --keypair argument (details below)
 - Run ssh-agent on local laptop and add the AWS keypair you're going to use.  For instance, on a Mac OX
 '''
 ssh-agent
 ssh-add ~/.ssh/bytecubed-test-keypair
 '''
 - Choose instance type. t2.micro is used for this setup but can be overridden with --instance_type argument
 - Choose region. us-east-2 is assumed but you can pass another region with --region
 - tag your instance with --tag (default is bytecubed-linux-test)
 - Name the security group you like to use or override by specifying --group to --extra-vars args as shown below.  Default is bytecubed-sg
 - Choose an AMI for the Linux instance by passing --ami (default is Amazon Linux in us-east-2, ami-922914f7)
 - Make sure AWS credentials for CLI access is setup.  Set the environment variable AWS_PROFILE to AWS profile you chose.  The ~/.aws/credentials file defines these profiles and should look like
```
[bytecubed]
region = us-east-2
aws_access_key_id  = XXXXXXXXXXXXXXXXXX
aws_secret_access_key = YYYYYYYYYYYYYYY
```
So, set the environment variable like:

export AWS_PROFILE=bytecubed

## Checkout Git repo
```
git clone https://github.com/madhujoshi/bytedocked
```
## Run the setup
```
cd bytedocked
ansible-playbook site.yml

If you want to override parameters, the command would be:
ansible-playbook site.yml --extra-vars "keypair=bytecubed-kp group=my-sg"
```

This should take about 5 minutes to complete the setup.  Find the DNS name of the instance the playbooks created and access it.  Paste them in your browser or use curl commands as shown below (replace the ec2 DNS name):

```
curl -I http://ec2-18-191-120-50.us-east-2.compute.amazonaws.com/
curl -k https://ec2-18-191-120-50.us-east-2.compute.amazonaws.com/
```
The first command will print 301 redirect and second command will print the challenge string.
