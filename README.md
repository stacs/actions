# LTS Team GitHub Actions

This repository contains GitHub actions that can be reused within our other repositories.
General descriptions of the actions are listed below; more information (such as what inputs, if any, they require) can be found within the action's YAML file.

* [maven-install.yaml](.github/workflows/maven-install.yaml): 
  Runs `mvn clean install` for a given maven project.
* [maven-deploy.yaml](.github/workflows/maven-deploy.yaml): 
  Runs `mvn deploy` in order to deploy the resources generated from [maven-install.yaml](.github/workflows/maven-install.yaml).
  This is generally only done for maven projects that use SNAPSHOT versioning.
* [maven-release.yaml](.github/workflows/maven-release.yaml):
  Runs `mvn release` for a given maven project to finalize the current version and prepare the maven project for the next version.
* [docker-build.yaml](.github/workflows/docker-build.yaml):
  Builds a docker image and pushes it to an ECR repository within the given AWS account.
  The docker image will be tagged with the short git commit hash, the branch name, and 'latest'.
* [docker-copy.yaml](.github/workflows/docker-copy.yaml):
  Pulls the docker image for a particular tool from the tool's ECR repository in one AWS account and pushes it to the tool's ECR repository in another AWS account.
  The workflow requires the image name from the 'old' ECR and then requires an image name for the 'new' ECR.
  The image name can be renamed when copying from the 'old' ECR to put into the 'new' ECR.
  The prod ECR is also immutable, so if a mistake has been made then a new version will have to be created and deployed.
* [aws-deploy.yaml](.github/workflows/aws-deploy.yaml):
  Updates the current task definition of an ECS service with a new image name taken from the ECR repo within an AWS account.
  The service will then be restarted so that the new image can be spun up as a running container.