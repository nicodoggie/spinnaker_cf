## Synopsis
Spinnaker is a Netflix open source, multi-cloud continuous delivery platform for releasing software changes with high velocity and confidence. It provides two core sets of features: cluster management and deployment management. For more information please refer to [Spinnaker main website](http://spinnaker.io/).

If you are interested in using Spinnaker on AWS, you can follow the instructions below to launch Spinnaker using CloudFormation or follow the official [Spinnaker Getting Started](http://spinnaker.io/) guide. 

AWS CloudFormation allows developers and systems administrators to create a collection of AWS resources as code in a template that can be provision repeatedly in an orderly and predictable fashion. 

In order to launch Spinnaker on AWS, you would need to create a an instance running Spinnaker in a VPC's private subnet, instance role, base role for instances that Spinnaker will launch, and policies and permissions. This CloudFormation templatis in this project takes care of the creation of most of that except for the base role. The base role (BaseIAMRole) is a static string name that Spinnaker uses to assign role to instances it creates (aka IAM:PassRole permission). This cannot be created by CloudFormation because in order to prevent IAM resources name collision, CloudFormation does not allow you to control the IAM resource name therefore a role/user/group/policy created using CloudFormation will have a random name.

![Alt text](images\spinnaker_architecture.png?raw=true "CloudFormation Template")

##Instruction - Using CloudFormation to Launch Spinnaker

Instructions to run Spinnaker using this Cloudformation:

1. Create an EC2 role - BaseIAMRole.
  Goto Console > AWS Identity & Access Management > Roles.
  Click Create New Role.
  Set Role Name to BaseIAMRole. Click Next Step.
  On Select Role Type screen, hit Select for Amazon EC2.
  Click Next Step.
  On Review screen, click Create Role.
  EC2 instances launched with Spinnaker will be associated with this role.
  
2. Create an EC2 Key Pair for connecting to your instances.
  Goto Console > EC2 > Key Pairs.
  Click Create Key Pair.
  Name the key pair my-aws-account-keypair. (Note: this must match your account name plus “-keypair”)
  AWS will download file my-aws-account-keypair.pem to your computer. chmod 400 the file.
  
3. Copy the [config file](config) file and place it in this folder ~/.ssh/ and replace <> with your instance info ---See sample in the repository
   
4. Copy the [spinnaker-tunnel.sh](spinnaker-tunnel.sh) file with the following content, and give it execute permissions

5.  Run Spinnaker on AWS using Cloudformation
   On AWS Console go to Cloudformation > Create new Stack
   Select the template file > Click Next
   Select the key previously created, enter password for a credential to be created, and optionally change VPC and subnet info if needed
   Accept IAM warning and click Submit
   
6. Execute the script to start your Spinnaker tunnel
    >./spinnaker-tunnel.sh start
    
    Open browser to http://localhost:9000 to access Spinnaker
    
    You can also stop your Spinnaker tunnel
    >./spinnaker-tunnel.sh stop
    
