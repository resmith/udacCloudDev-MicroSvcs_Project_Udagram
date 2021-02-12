# Udacity Cloud Developer - Udagram Project - MicroServices 
This repository is for the Udacity Cloud Developer - Microservices project.

## Objective
- Split an application into Microservices
- Build a docker image
- Create a reverse proxy
- Utilize Kubernetes for container orchestration


## Project files
This repository is a *meta* repository. The split of components into microservices led to their being their own git repository.

The git repositories for the microservices for this project are:

[FrontEnd](https://github.com/resmith/udacCloudDev-MicroSvcs_Project_Udagram_Frontend)

[Reverse Proxy](https://github.com/resmith/udacCloudDev-MicroSvcs_Project_Udagram_ReverseProxy)

[API Feed](https://github.com/resmith/udacCloudDev-MicroSvcs_Project_Udagram_API_Feed-)

[API User](https://github.com/resmith/udacCloudDev-MicroSvcs_Project_Udagram_API_User)


## For deployment.yaml evaluation:
- [kubeval](https://www.kubeval.com/installation/)
- [kube-score](https://github.com/zegl/kube-score)


## Prerequisites - Environment Variables
The environment variables are set locally using a *0_set_env.sh* file. This file is called from the different shell scripts that are part of the build/test/deploy process. 

*Note*
sh can't be used to call *0_set_env.sh*  from within the scripts  because it will set the variables for the session (when set_env.sh is executed) and those variables will be gone after execution is completed. The setting of the enviornment variables should be called as *. ./0_set_env.sh*. 

## Example Script that calls *0_set_env.sh*
```
export APP_NAME=udagram-api-feed
export DOCKER_IMAGENAME=theresmith/$APP_NAME
export PORT=8080
export DOCKER_PORT=49160
export BASE_URL=http://localhost
export URL=$BASE_URL:$PORT
export API_ENDPOINT=/api/v0/feed

# *** AWS CLOUD INFO
export POSTGRES_USERNAME=udagramapp
export POSTGRES_PASSWORD=<Password>
export POSTGRES_HOST=<Postgre HostName>
export POSTGRES_DB=postgres
export AWS_BUCKET=<bucket>
export AWS_REGION=<region>
export AWS_PROFILE=default
export JWT_SECRET=<secret>

```

## Example Script that calls *0_set_env.sh*
```
. ./0_set_env.sh
#
# In case Docker image is changed
# otherwise kube will say 'unchanged' because of no change to deployment.yaml
kubectl delete -f ./deployment.yaml
#
kubectl apply -f ./deployment.yaml
kubectl apply -f ./service.yaml
#
```

### Testing Db connection using Postgres SQL

This is the most direct way to test to ensure the Db is accessible when developing / testing locally.

1. Install Postgres / PSQL

2. Run the below command, updating it the variables with the appropriate values

*note* the password will be prompted for at execution time
```
psql \
   --host=udagramdb.xyxasdada.us-west-2.rds.amazonaws.com \
   --port=5432 \
   --username=udagramappuser \
   --password \
   --dbname=udagramdb 
```


## Docker Build
The Docker build uses a multi-stage build to keep the size of the docker image small.

[Docker Multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)


## Reference

[Check env variables are set](https://stackoverflow.com/questions/307503/whats-a-concise-way-to-check-that-environment-variables-are-set-in-a-unix-shell/307735)



