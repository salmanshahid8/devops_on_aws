# Module 2


## Testing and automation
- code build docker images can be found on [aws codebuild docker images](https://gitub.com/aws/aws-codebuild-docker-images)
- While there are many kinds of application tests, the three of them are:
    - Regression testing - Tests to ensure that previously developed applications don’t break with new changes
    - Integration testing - Tests to ensure separately developed modules work together as expected
    - Unit testing - Tests a specific section of code to ensure the code does what it is expected to do

## Automate testing
- AWS CodeBuild will provision a clean and consistent environment to perform various application tests. You can also create reports in CodeBuild that contain details about tests that are run during builds. 

## AWS CodePipeline
- You can automate your release process by using AWS CodePipeline to test your code and run your builds with CodeBuild. 
- CodePipeline is a continuous delivery (CD) service that you can use to model, visualize, and automate the steps required to release your code. This process includes building your code. A pipeline is a workflow construct that describes how code changes go through a release process.
