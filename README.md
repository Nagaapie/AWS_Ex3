# Ex3 Instruction

Project has four parts:

1. [The RestAPI Feed Backend](/udacity-c3-restapi-feed), a Node-Express feed microservice.
2. [The RestAPI User Backend](/udacity-c3-restapi-user), a Node-Express user microservice.
3. A Nginx reverse proxy that dispatches requests to feed & user services
4. [The Simple Frontend](/udacity-c3-frontend) A basic Ionic client web application which consumes the RestAPI Backend. 


# Steps that have been made
1. Install Travis plugin to github
2. Create Postgres DB in AWS
3. Create S3 bucket and enable CORS
4. Build Frontend and Backend services and push to docker hub
5. Run images locally with: docker-compose up
6. Create EKS cluster and config: aws eks --region <region> update-kubeconfig --name <cluster name>
7. Check EKS config: "kubectl get pod"
8. Apply AWS secrets in profile file
9. Deploy images to cluster: kubectl apply -f "config deploy yaml"

# credentials
First, you need to configure your variables in the next three files on the `deployments/k8s` directory:

- env-secret.yaml
- aws-secret.yaml
- env-configmap.yaml

# docker
To test the application in local with docker, you need to run the next commands:

```bash
docker-compose -f docker-compose-build.yaml build
docker-compose up

#This will build the necessary images and then it will mount the application in local. You need to define the next variables in local, in order to run it correctly:

- POSTGRESS_USERNAME
- POSTGRESS_PASSWORD
- POSTGRESS_DB
- POSTGRESS_HOST
- AWS_REGION
- AWS_PROFILE
- AWS_BUCKET
- JWT_SECRET

#you can get the images from my docker hub account:

- https://hub.docker.com/repository/docker/andreladeira/reverseproxy
- https://hub.docker.com/repository/docker/andreladeira/udagram-frontend
- https://hub.docker.com/repository/docker/andreladeira/udagram-user
- https://hub.docker.com/repository/docker/andreladeira/udagram-feed

###NOTES

# Installing Node and NPM
This project depends on Nodejs and Node Package Manager (NPM). Before continuing, you must download and install Node (NPM is included) from [https://nodejs.com/en/download](https://nodejs.org/en/download/).

# Installing Ionic Cli
The Ionic Command Line Interface is required to serve and build the frontend. Instructions for installing the CLI can be found in the [Ionic Framework Docs](https://ionicframework.com/docs/installation/cli).

# Installing project dependencies

This project uses NPM to manage software dependencies. NPM Relies on the package.json file located in the root of this repository. After cloning, open your terminal and run:
```bash
npm install
```
>_tip_: **npm i** is shorthand for **npm install**

# Setup Backend Node Environment
You'll need to create a new node server. Open a new terminal within the project directory and run:
1. Initialize a new project: `npm init`
2. Install express: `npm i express --save`
3. Install typescript dependencies: `npm i ts-node-dev tslint typescript  @types/bluebird @types/express @types/node --save-dev`
4. Look at the `package.json` file from the RestAPI repo and copy the `scripts` block into the auto-generated `package.json` in this project. This will allow you to use shorthand commands like `npm run dev`


# Configure The Backend Endpoint
Ionic uses enviornment files located in `./src/enviornments/enviornment.*.ts` to load configuration variables at runtime. By default `environment.ts` is used for development and `enviornment.prod.ts` is used for produciton. The `apiHost` variable should be set to your server url either locally or in the cloud.

# Running the Development Server
Ionic CLI provides an easy to use development server to run and autoreload the frontend. This allows you to make quick changes and see them in real time in your browser. To run the development server, open terminal and run:

```bash
ionic serve
```

# Building the Static Frontend Files
Ionic CLI can build the frontend into static HTML/CSS/JavaScript files. These files can be uploaded to a host to be consumed by users on the web. Build artifacts are located in `./www`. To build from source, open terminal and run:
```bash
ionic build
```

# kubernetes
Once you have connected to kubernetes cluster, you need to appy the configurations in the `/deployments/k8s/` directory for all the deployments and services (remember to run first frontend, feed and user, before running reverseproxy):

- kubectl apply -f <service-file-names>
- kubectl apply -f <deployment-file-names>
- kubectl get pods

# travis
In the root directory of the github repository, there is a `.travis.yml` that build all the containers and test that is working as expected.
