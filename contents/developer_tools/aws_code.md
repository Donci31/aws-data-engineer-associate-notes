# AWS Code

- [AWS CodeDeploy](aws_code/aws_code_deploy.md)

- [AWS CodeBuild](aws_code/aws_code_build.md)

- [AWS CodeCommit](aws_code/aws_code_commit.md)

- [AWS CodePipeline](aws_code/aws_code_pipeline.md)

## Comparison of *AWS CodeDeploy*, *AWS CodeBuild*, *AWS CodeCommit*, and *AWS CodePipeline*

| Feature                         | **AWS CodeDeploy**                | **AWS CodeBuild**                 | **AWS CodeCommit**                | **AWS CodePipeline**               |
|----------------------------------|-----------------------------------|-----------------------------------|-----------------------------------|------------------------------------|
| **Service Type**                 | Deployment Automation             | Build Automation                  | Source Control                    | CI/CD Orchestration                |
| **Primary Use**                  | Deploy applications to EC2, Lambda, or On-Premises | Compile, test, and package code | Secure Git repositories           | Automate build, test, and deployment pipeline |
| **Integration with Other AWS Services** | Integrates with CodePipeline, Lambda, and EC2 | Integrates with CodePipeline, CodeDeploy | Integrates with CodePipeline, CodeBuild | Integrates with CodeCommit, CodeBuild, CodeDeploy |
| **Customization**                | Custom deployment hooks and strategies | Buildspec.yml for customizing build process | Versioning and access control | Custom workflows and conditional execution |
| **Payment Model**                | Pay-as-you-go based on usage      | Pay-per-build based on build time | Pay-as-you-go based on storage and usage | Pay-per-pipeline execution         |
| **Target Use Cases**             | Automated application deployment | Automated build and testing       | Private Git repositories          | End-to-end CI/CD automation        |
