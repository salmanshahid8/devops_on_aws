# Module 2

## The Importance of Operation in CI/CD Pipelines
Continuous integration and continuous delivery (CI/CD) has become a key component of the software development lifecycle and is the backbone of DevOps. The need for application patching and deployments in monolith and legacy environments are still a reality for multiple enterprises.

Sometimes, you might be able to launch new environments, especially if you are working with deployment strategies such as Blue/Green deployment (which is explained in the second course of the series). Sometimes, however, you do not have the ability of create brand new environments, and must work with existing ones.

Releasing new features into production by changing a current environment could be challenging because of application complexity, external dependencies, and the coordination of multiple teams in a chain of procedures. 
AWS Systems Manager
 enables the automation of operational tasks. Systems Manager receives the list of instances to be patched from AWS CodeBuild, and then it remotely runs the commands in all target instances.

Systems Manager provides a unified user interface so you can track and resolve operational issues across your AWS applications and resources from a central place. Systems Manager simplifies resource and application management, shortens the time to detect and resolve operational problems, and makes it easier to operate and manage your infrastructure at scale.

Some of its benefits include:

Reduces the time needed to resolve operational issues

Offers easy-to-use automation

Improves visibility and control

Manages hybrid environments

Helps maintain security and compliance

Connects with IT service management (ITSM) or IT operations management (ITOM) software

Using Run Command, a capability of AWS Systems Manager, you can remotely and securely manage the configuration of your managed instances. A managed instance is any Amazon Elastic Compute Cloud (Amazon EC2) instance or on-premises machine in your hybrid environment that has been configured for Systems Manager. Use Run Command to automate common administrative tasks and perform one-time configuration changes at scale. You can use Run Command from the AWS Management Console, the AWS Command Line Interface (AWS CLI), AWS Tools for Windows PowerShell, or the AWS SDKs. Run Command is offered at no additional cost.


## Most Common Third-party Services and Tools for DevOps Operations
Though AWS offers you a suite of services for DevOps operations and monitoring, third-party, open-source tooling is also well known and widely used by the DevOps community.

The following list describes some of the most common open-source DevOps tooling available. In addition, both Puppet and Ansible are members of the AWS Partner Network (APN). There are also many other very good tooling and services for DevOps operations available!

Puppet

Puppet is an infrastructure automation and delivery tool. Puppet is also responsible for publishing one of the most important research papers in the DevOps industry—the famous “State of DevOps” paper, which can be found here: 
State of DevOps Report 2021 | Puppet
.

Jenkins

Jenkins is an open-source automation server. Jenkins provides many plugins to support building, deploying, and automating any project. It’s common to have Jenkins working together with AWS CodePipeline because they developed a 
plugin
 that works with CodePipeline. For more information, see: 
Setting up a CI/CD pipeline by integrating Jenkins with AWS CodeBuild and AWS CodeDeploy | AWS DevOps Blog (amazon.com)


Ansible

Ansible, which is sponsored by Red Hat, is also used for IT automation. 

Although this course focuses on AWS products and services, being familiar with open-source solutions can be helpful, especially when you need a multi-cloud solution. When you plan your DevOps operations, it’s important to evaluate what AWS solutions and third-party tools make sense as part of your pipeline.