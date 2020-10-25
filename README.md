# egeoffrey-actions

A collection of supporting Github Actions for eGeoffrey.

## Build Action (build.yml)

This workflow is triggered at every push and pull-request and automate the process of:

1. Testing an eGeoffrey module code
1. Packaging it in a Docker image
1. Publishing it to Dockerhub for distribution

### How to use it

- Create a DockerHub account
- Create a DockerHub [Access Token](https://docs.docker.com/docker-hub/access-tokens/)
- In the Github repository where your eGeoffrey package resides, go to "Settings", "Secrets" and create a secret called `DOCKERHUB_USERNAME` storing your DockerHub username and `DOCKERHUB_TOKEN` storing your DockerHub access code
- Commit your code as you would normally do with `egeoffrey-cli commit "new change"`. The `egeoffrey-cli` utility will automatically download and keep up to date the workflow file in your repository
- As the new version is committed and pushed to your repository, the build workflow automatically triggers. You can see the result of the execution under "Actions". In case of failure, you will receive an email notification
- If everything goes well, the eGeoffrey package is automatically built and pushed to your DockerHub repository

### How it works

Every time `egeoffrey-cli commit` is run, it will download the latest version of this GitHub Action workflow and deploy it in your local repository, just before committing the changes and pushing them to GitHub. This makes the workflow trigerring as the new code is pushed to GitHub.

The worklof does the following:

1. Starts up a virtual image
1. Checkout the latest version of the code
1. Login to DockerHub
1. Setup Docker for cross-platform building
1. Install eGeoffrey
1. Build your eGeoffrey Package 
1. Install your package
1. Wait a few seconds for collecting enough logs
1. Check if there are no errors showing up in the logs
1. Push the eGeoffrey Package to DockerHub

If any of the above steps fails, the entire workflows fails.
