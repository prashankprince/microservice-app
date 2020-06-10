# Microservice application

A web application/microservice app comprising of several components communicating to each other.

## Components

1. Frontend part , user-interface api provides User Interface 
2. AWS API gateway can be used by which web app can communicate with services which are internal APIs talking to each other like db
3. Authentication API , auth-api used to authenticate user using outh2 model. 
4. Public end points can be defined in AWS route 53.
5. Database, db-api is used for database
6. TODO api is used to create , delete operation in redis queue which is later processed by log-message
7. log-message, is a queue processor to read from Redis queue and print them.
8. Deploy kubernetes cluster in AWS or EKS and Then run 
 ``kubectl create -R -f k8s/``

## Architecture

![image](https://user-images.githubusercontent.com/48621845/84292101-90f04980-ab63-11ea-97bd-2732b272b429.png)


## CI CD Plan

Each team can build and deploy the services that it owns independently, without affecting or disrupting other teams.

Before a new version of a service is deployed to production, it gets deployed to dev/test/QA environments for validation. Quality gates are enforced at each stage.

A new version of a service can be deployed side by side with the previous version.

Using Jenkins X which is a cloud native CI/CD approach, we can have end to end pipeline created for the application present in Github.
Jenkins X will constantly monitor github using webhook following GitOps approach and as soon as a code is checked in by a team member for a microservice , pipeline is triggered which build the code, does unit testing, create docker images, push it to docker repo , create helm charts and pushes it to chartmusuem repo. Using charts , it deploys to staging environment(pods created in staging namespace) and gives a test UI which when validated is pushed to staging environment. 
Once satisfied with code change , we can promote to production using canary deployment/blue-green deployment.

## Jenkins X architecture:

![image](https://user-images.githubusercontent.com/48621845/84293965-1d9c0700-ab66-11ea-8948-b22753c52dbd.png)

Ultimately achieving 

![image](https://user-images.githubusercontent.com/48621845/84294164-6a7fdd80-ab66-11ea-84bb-116c9274d0a2.png)
