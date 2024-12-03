# Module 1

## Build
- A build is going to take your source, compile it, and retrieve dependency packages from a repository—like Node package manager (npm) modules or an Apache Maven artifact in Java. A build will usually include automated testing to check the quality of the code, and unit tests to exercise the code and make sure it does what you expect it to. 

## AWS CodeBuild
- It is a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy. You don’t need to provision, manage, and scale your own build servers. CodeBuild scales continuously and can process multiple builds concurrently.
- A branch in Git is a pointer to a commit. When you make commits in a branch, the pointer automatically moves forward. The main idea behind the Feature Branch Workflow is that all feature development takes place in a dedicated branch. This workflow makes it easy for multiple developers to work on a feature without disrupting the main code base. When working in source control, you and your team need to agree on a convention that allows you to work on features and keep code out of the main branch until you are confident that it is ready for production. 

## Test
- Next, we talked about testing—and how testing fits into a continuous integration and continuous delivery (CI/CD) pipeline.  By performing testing at different stages of your development cycle, you are creating a measure of quality.  When you write code, that code is going to do exactly what you tell it to do. The important question is: Are you telling it to do the right thing? At this point, the application is under source control and you have automated tests to make sure the code is in a good state. Because you want to run these tests each time you commit code, you will receive immediate feedback whether the code committed still works.

## AWS CodePipeline 
- It is is a fully managed, continuous delivery service that helps you automate your release pipelines for fast and reliable updates to your application and infrastructure. Based on the release model you define, CodePipeline automates the build, test, and deploy phases of your release process each time there is a code change. You can use CodePipeline to deliver features and updates both rapidly and reliably.
- The previous course worked with serverless compute, but this course will be server-based. The readings will have information about deployments using serverless technologies, if you’re interested in learning more. Code repositories—and tools for build and test—are still relevant for server-based workloads. 

## Deployment Strategies for Serverless
Terminology
To understand the deployment strategies for serverless applications, we will first cover the terminology of versions, aliases, and traffic shifting. Each AWS Lambda function can have any number of versions and aliases associated with them.

Versions are snapshots of a function that includes the code and configuration, and it is a good practice to publish a new version each time you update your function code. When you invoke a specific version (using the function name and version number combination) you will get the same code and configuration regardless of the state of the function. This protects you against accidentally updating production code. To use versions, you should create an alias, which is a pointer to a version.

Aliases have a name and an Amazon Resource Number (ARN) similar to the function and are accepted by the Invoke APIs. If you invoke an alias, Lambda will in turn invoke the version that the alias is pointing to. In production, you would first update your function code, publish a new version, and invoke the version directly to run tests against it. After you are satisfied, you would change the alias to point to the new version.

Traffic shifting can shift incoming traffic between two versions of a Lambda function based on pre-assigned weights. You can use this feature to gradually shift traffic between two versions, helping you reduce the risk of new Lambda deployments. You can also change your Lambda function’s code without affecting other upstream dependencies that rely on the alias.

Deploying serverless applications
If you use AWS SAM to create your serverless application, it comes built-in with AWS 
CodeDeploy
 to provide gradual Lambda deployments. With a few lines of configuration, AWS SAM does the following for you:

Deploys new versions of your Lambda function, and automatically creates aliases that point to the new version.

Gradually shifts customer traffic to the new version until you're satisfied that it's working as expected, or you roll back the update.

Defines pre-traffic and post-traffic test functions to verify that the newly deployed code is configured correctly and your application operates as expected.

Rolls back the deployment if Amazon CloudWatch alarms are generated.

Deployment options
Now that you know about AWS SAM, you can learn about your deployment options. The following list describes other traffic-shifting options that are available:

Canary: Traffic is shifted in two increments. You can choose from predefined canary options. The options specify the percentage of traffic that's shifted to your updated Lambda function version in the first increment, and the interval, in minutes, before the remaining traffic is shifted in the second increment.

Linear: Traffic is shifted in equal increments with an equal number of minutes between each increment. You can choose from predefined linear options that specify the percentage of traffic that's shifted in each increment and the number of minutes between each increment.

All-at-once: All traffic is shifted from the original Lambda function to the updated Lambda function version at once.

An all-at-once deployment will shift traffic instantly from one version to another, while canary and linear, are much safer, more gradual deployment options.


## Appsecfile
The application specification file (AppSpec file) is file used by AWS CodeDeploy to manage a deployment. It can be written in 
YAML
 or JSON.

AppSec file for Amazon ECS applications
For Amazon Elastic Container Service (Amazon ECS) applications, the AppSpec file is used by AWS CodeDeploy to determine:

Your Amazon ECS task definition file. This file is specified with its Amazon Resource Number (ARN) in the TaskDefinition instruction of the AppSpec file.

The container and port in your replacement task set where your Application Load Balancer or Network Load Balancer reroutes traffic during a deployment. This setting is specified in the LoadBalancerInfo instruction of the AppSpec file.

Optional information about your Amazon ECS service, such the platform version it runs on, its subnets, and its security groups.

Optional Lambda functions to run during hooks that correspond with lifecycle events during an Amazon ECS deployment.


Here is an example of an AppSpec file for deploying an Amazon ECS service, written in YAML.

version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: "arn:aws:ecs:us-east-1:111222333444:task-definition/my-task-definition-family-name:1"
        LoadBalancerInfo:
          ContainerName: "SampleApplicationName"
          ContainerPort: 80
# Optional properties
        PlatformVersion: "LATEST"
        NetworkConfiguration:
          AwsvpcConfiguration:
            Subnets: ["subnet-1234abcd","subnet-5678abcd"]
            SecurityGroups: ["sg-12345678"]
            AssignPublicIp: "ENABLED"
        CapacityProviderStrategy:
          - Base: 1
            CapacityProvider: "FARGATE_SPOT"
            Weight: 2
          - Base: 0
            CapacityProvider: "FARGATE"
            Weight: 1
Hooks:
  - BeforeInstall: "LambdaFunctionToValidateBeforeInstall"
  - AfterInstall: "LambdaFunctionToValidateAfterInstall"
  - AfterAllowTestTraffic: "LambdaFunctionToValidateAfterTestTrafficStarts"
  - BeforeAllowTraffic: "LambdaFunctionToValidateBeforeAllowingProductionTraffic"
  - AfterAllowTraffic: "LambdaFunctionToValidateAfterAllowingProductionTraffic"

AppSpec file for AWS Lambda applications
For Lambda applications, the AppSpec file is used by CodeDeploy to determine:

Which Lambda function version to deploy.

Which Lambda functions to use as validation tests.

For Lambda, you must specify your application alias, and the current and target version you’re deploying to.

Here is an example of an AppSpec file for deploying a Lambda function version, written in YAML.

version: 0.0
Resources:
  - myLambdaFunction:
      Type: AWS::Lambda::Function
      Properties:
        Name: "myLambdaFunction"
        Alias: "myLambdaFunctionAlias"
        CurrentVersion: "1"
        TargetVersion: "2"
Hooks:
  - BeforeAllowTraffic: "LambdaFunctionToValidateBeforeTrafficShift"
  - AfterAllowTraffic: "LambdaFunctionToValidateAfterTrafficShift"

Hooks
The content in the 'hooks' section of the AppSpec file varies, depending on the compute platform for your deployment. The 'hooks' section for an Amazon EC2 or on-premises deployment contains mappings that link deployment lifecycle event hooks to one or more scripts. The 'hooks' section for a Lambda or an Amazon ECS deployment specifies Lambda validation functions to run during a deployment lifecycle event. If an event hook isn’t present, no operation is run for that event. This section is required only if you are running scripts or Lambda validation functions as part of the deployment.

Permissions
The 'permissions' section specifies how special permissions (if any) should be applied to the files and the directories in the 'files' section after they are copied to the instance. You can specify multiple object instructions. This section is optional. It applies to Amazon Linux, Ubuntu Server, and RHEL instances only.

Note: The 'permissions' section is used only for Amazon EC2 or on-premises deployments. It’s not used for Lambda or Amazon ECS deployments.

## Create your own AWS account
As part of this course, there are hands-on exercises where you will have the opportunity to use and explore a variety of AWS services.  To work on the hands-on exercises, you must have your own AWS account. If you don’t already have an AWS account, see the following section for more information about how to get started.

Important: Not all hands-on exercises are designed to run within the AWS Free Tier. Some hands-on exercises might have a cost associated with them. Check the 
AWS pricing page
 to determine the cost of the referenced AWS services. 

AWS Free Tier
Explore more than 100 products and start building on AWS using the AWS Free Tier. Three different types of free offers are available, depending on the product you use. See the following list for details about each product.

Always free - These AWS Free Tier offers do not expire and are available to all AWS customers.

12 months free - Enjoy these offers for 12 months following your initial AWS signup date

Trials - Short-term, free-trial offers start from the date you activate a particular service

To learn more, see: 
AWS Free Tier

Don’t have an AWS account?
To create an AWS account, see: 
How do I create and activate a new AWS account?
 If you already have an account, you should be ready to work on the exercises.

## Deploying Updates to Lambda with SAM and CodeDeploy
If you are running a serverless application that uses AWS Lambda, you might need to address additional considerations when you deploy updates to your Lambda functions. As an example, dependencies between service resources could cause issues for your serverless deployment if the various resources that your function needs aren’t created in the correct order. 

To help with serverless deployments, AWS offers the AWS Serverless Application Model (AWS SAM), which is an open-source framework that you can use as a scaffold for organizing your serverless resources. When you create or update an AWS SAM template, you use YAML and a shorthand syntax specific to AWS SAM to model your application. With AWS SAM, you can use code to define your application’s functions and other resources, such as APIs or event-source mappings. When you deploy your application, AWS SAM expands its syntax into AWS CloudFormation syntax. By turning your serverless infrastructure into code, you can build and deploy your serverless applications more quickly and reliably.

Say that you have been working on a new version of a Lambda function for your application. The updated function has passed all your tests, and you’re now ready to deploy the new version. After deployment, you want production traffic to gradually shift from the older version of the function to the new version so you can monitor how it performs. 

You could deploy the new version of the function (and all its dependencies) through the console or the AWS Command Line Interface (AWS CLI), but doing so can be a very manual process. Instead of going the manual route, you could deploy the DevOps way, using AWS SAM and CodeDeploy to both deploy the new version and automatically shift production traffic to it.

To do so, edit the AWS SAM template to configure the necessary CodeDeploy settings. By including the AutoPublishAlias: live setting in the AWS SAM definition for your Lambda function, CodeDeploy deploys a new version of the Lambda function. After it deploys the function, it also creates an alias called live that points to the new version, which CodeDeploy will use to shift traffic between the two versions. 

To specify which deployment model you want to use, add a DeploymentPreference section to your template’s function definition. The deployment options include:

Canary - This option shifts traffic in two increments spaced by an interval (in minutes). You can use predefined options to specify the percentage of traffic shifted in the first increment and the duration of the interval before CodeDeploy shifts the remaining traffic. For example, Canary10Percent30Minutes will send 10 percent of traffic to the new version and 30 minutes later, it will complete the deployment by sending all traffic to the new version.

Linear - This option shifts traffic in equal increments spaced by an equal number of minutes. You can use predefined options to specify the percentage of traffic shifted in the increments, and the duration of the interval between each increment. For example, Linear10PercentEvery10Minutes will continuously shift 10 percent of traffic to the new version every 10 minutes, until 100 percent of the traffic gets sent to the new version.

All-at-once - This option shifts all traffic from the old version to the new version.

You can also use the DeploymentPreference section to configure additional deployment resources. Examples include Amazon CloudWatch alarms that monitor for deployment errors, or hooks that test whether the Lambda function is performing as expected before or after any traffic shifting occurs.

Here’s an example AWS SAM template that defines the deployment for a Lambda function:
MyLambdaFunction:
  Type: AWS::Serverless::Function
  Properties:
    Handler: index.handler
    Runtime: nodejs12.x
    AutoPublishAlias: live
    DeploymentPreference:
      Type: Linear10PercentEvery10Minutes
      Alarms:
        # A list of alarms that you want to monitor
        - !Ref AliasErrorMetricGreaterThanZeroAlarm
        - !Ref LatestVersionErrorMetricGreaterThanZeroAlarm
      Hooks:
        # Validation Lambda functions that are run before & after traffic shifting
        PreTraffic: !Ref PreTrafficLambdaFunction
        PostTraffic: !Ref PostTrafficLambdaFunction
      # Provide a custom role for CodeDeploy traffic shifting here, if you don't supply one
      # SAM will create one for you with default permissions
      Role: !Ref IAMRoleForCodeDeploy # Parameter example, you can pass an IAM ARN

With your CodeDeploy settings in the AWS SAM template, you can deploy your new version (for example, by using the AWS SAM CLI). To see the application traffic shift from the old version and the new one as you specified in real time, you can use either the CodeDeploy console or the Lambda console. You could also perform tests or view logs to check whether the new deployment is working as expected. And if you need to update your Lambda function in the future, you can apply DevOps practices and use AWS SAM and CodeDeploy to redeploy and monitor your updates in an automated, repeatable, and reliable way.

## 