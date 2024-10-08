# Project overview

This project is a coworking analytics application system contain PostgresSQl as a database and a Flask webserver.
The system is built locally using docker then connect to a local database managed by Kubernetes.
The whole build and deployment process, we all leverage on AWS servces with services: CodeBuild, ECR, EKS, CouldWatch



# build and deployment process

## creat a cluster

we create a cluster at us-east-1 with node type is t3.small, min node is 1 and max is 2. we also create a user in aws console with enough permission to work with EKS

## deploy postgres

PostgresSQL is deployed into eks cluster by deployment\postgresql-deployment.yaml
The Postgres service is using file C:\Users\Admin\Documents\Github\cd12355-microservices-aws-kubernetes-project-starter\deployment\postgresql-service.yaml

## deploy coworking Flask application

The application is testing runing in local machine, connected with DB managed in EKS by using kube port-forwarding.
we create a docker file do dockerize The application. which also be tested when runing in docker as local machine
then, we config CodeBuild connect with Github to automatically trigger build for new commit. the built image then will be push to AWS ECR


Flask server is deployed into eks cluster by deployment\co-working-app-deployment.yaml combine with configMap and secret at deployment\co-working-configmap.yaml and deployment\co-working-secret.yaml

## monitoring 

we enable cloudwatch for our cluster by using amazon-cloudwatch-observability with below cmd
aws eks create-addon --addon-name amazon-cloudwatch-observability --cluster-name my-cluster-name
