#Task:

Setting up a Multibranch pipeline and regular Jenkins pipelines with manual or auto triggers for deploying an application.

Objective:
The objective of this task is to train systems engineers to set up a Multibranch pipeline and a Manual pipeline for deploying an application with different ports depending on envs and changing logo.svg files depending on envs (branch name) as well. The engineers will learn how to create two branches - main and dev, in GIT these branches will be in envs role as well, configure stages for checkout, build, test, build docker image, and deploy, and make changes to the picture and ports for each branch.

#Details:

After triggering main or dev branch you have such stages as checkout – build – test – build docker image – deploy. You must change the picture "logo.svg" for main and dev branches. You can put any pictures you want there but in the .svg format. You should see the difference after application deployment. Also, you should change ports, for the main branch it is 3000 and 3001 for dev as well. It looks like http//localhost:3000 or http://localhost:3001. You perform change of logo.svg in corresponding branches dev and main and you should add conditional logic to your pipeline to set port number depending on branch.

Global goal is creation of two Jenkins pipelines. The first is a multibranch pipeline called "CICD", the second one is a regular pipeline called "CD_deploy_manual" and it should be manually triggered to execute deployment. You should pull this repo - https://github.com/epam-msdp/cicd-pipeline to your local machine, then create your own repo on GitHub and push files contained inside this repo, after this you must create a new branch called dev and, in the end, you will see two branches "main" and "dev"

#Steps:

Click the arrows to see more information.

1. Configure global tools configuration Node 7.8.0.
2. Create multibranch and one regular pipeline.
3. Configure GIT.
4. Set up scan multibranch pipeline every 1 min. (Ignore it if you have a public IP and you can set up webhooks in GitHub - it is much better way).
5. Configure role base access. Create three groups (dev, qa, devops and grant different access rights) for example devops can do anything with Jenkins. Dev team can just run jobs and QA can just read logs in the pipelines but can’t run.
6. Configure pipeline and stages of CICD pipeline.
   * The Jenkinsfile should be the same for main and dev branches.
   * Setup your NodeJS platform version in “global tools configuration”.
   * Set up GitHub ssh creds for Jenkins.
   * Command for build of NodeJS application - npm install.
   * Command for testing of NodeJS application - npm test.
   * Build docker images:
     docker build -t nodemain:v1.0 for main
     docker build -t nodedev:v1.0 for dev.
   * Before you run your container you should stop and delete all previously running containers. Try to make the lowest downtime.
   * Run your application in docker container:
     for main branch docker run -d –expose 3000 -p 3000:3000 nodemain:v1.0
     for dev branch docker run -d –expose 3001 -p 3001:3000 nodedev:v1.0.
7.  "Create a new pipeline called CD_deploy_manual, which should be used for manually deploying to either the 'main' or 'dev' environment. Since we do not have separate environments, 'main' and 'dev' branches are used to imitate real environments with different application versions. This pipeline should include two parameters:
   * 'main/dev', to specify the target environment
   * 'Image tag', to specify the tag of the Docker image to be deployed."
