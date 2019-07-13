This project was originally published in [Deploying a scalable web application with Docker and Kubernetes](https://medium.com/@joycelin.codes/deploying-a-scalable-web-application-with-docker-and-kubernetes-a5000a06c4e9) from the [Postman Engineering blog](https://medium.com/postman-engineering).

### Pre-requisites for local development
1. Download and install [Node.js](https://nodejs.org/en/) and a package manager like [npm](https://www.npmjs.com/), and
1. A container platform like [Docker](https://www.docker.com/get-started). Remember to start Docker on your machine.

### For Development

    npm install // install dependencies
    npm start // start app on 3000
    npm run start-server // start server on 5500
    
### Steps to Build
  
  ```
     docker login                  // verified I could log into Docker
     git clone <cat-app-ui-repo> 
     cd <cat-app-ui-repo>
     cp .env.sample .env           // create a .env file from example and added my docker username to entry
     npm install
     npm audit fix                 // fix the audit errors that can be fixed automatically
     npm audit                     // find out details for steps to fix these manually
  ```
  
 ### Steps to Launch Cat UI
 ```
     npm start
  ```
  
### Steps to Launch Cat Express Server
  ```
   npm run start-server           // Start the Express Server on port 5500
   ```
  
  
  Whenever you or someone else wants to add new dependencies to the project you perform npm install --save or npm install --save-dev. Then package.json is automatically updated, and needs to be committed again.
### For Production

1. **Environment variables**: Create a new file called `.env` located in the root of your project directory. Add your production domain and docker hub username. See the example in `.env.example`. 
1. **API tests**: If you plan to run Postman tests, then update the `bin/deploy.sh` file by selecting a method of running your Postman tests. To run either of the last 2 options, update your Postman collection UID and environment UID in `bin/deploy.sh` and also add your [Postman API](https://docs.api.getpostman.com/) key to the `.env` file you created in the previous step.
1. **Describe deployment**: Update the `deployment-prod.yaml` file to describe your backend deployment. Update the `deployment-ui-prod.yaml` file to describe your frontend deployment. These files will include your container image, resource allocation, the desired number of replicas, and other important information.
1. **Deploy**: Run the deployment script
    
`npm run deploy // run API tests, then deploy frontend and backend to production`

For the deployment, I used a hosted Kubernetes provider called [Kubesail](https://kubesail.com/) that creates a free managed namespace. However, the underlying deployment utility [`npx deploy-to-kube`](https://github.com/kubesail/deploy-to-kube) supports any Kubernetes cluster. By running it inside your app's directory, this utility will automatically generate a Dockerfile (if it doesn't exist), build and push deployment images, generate Kubernetes configuration files (if it doesn't exist), and trigger a deployment on your Kubernetes cluster. 

![[catURL website](https://github.com/postmanlabs/node-doc-kube/blob/master/catUrl.gif)](https://github.com/postmanlabs/node-doc-kube/blob/master/catUrl.gif)
