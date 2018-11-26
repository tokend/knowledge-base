# Docker image replacement

Imagine, there are new features coming from Front/Back-End Development teams, but no DevOps engineer around. Assume you have access to deploy machine and etc. You can deploy those features by yourself, just follow these steps:

 1. > docker login registry.example.com - to login to the Container Registry using your GitLab credentials
 1. > docker-compose down - to stop environment
 2. Edit in **docker-compose.yaml** required revisions

For example, to deploy latest web-client, replace branch-name and revision

    web_client:
        image: registry.gitlab.com/tokend/<branch-name>:<revision>

For core & horizon, dont forget to add **pg-**

    horizon:
        image: registry.gitlab.com/tokend/horizon:pg-<revision>
  4. > docker-compose up -d - to start environment with newly attached containers in the background 