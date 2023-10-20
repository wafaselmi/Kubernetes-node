# Minikube Setup for Windows:
Pre-requisites : Docker Desktop - Kubectl - Minikube
start Minikube :
`minikube start --driver=hyperv`
Check Cluster :
`kubectl get nodes`
Clone Repo :
`git clone https://github.com/wafaselmi/Kubernetes-node.git`
Deploy to minikube :
`minikube docker-env | Invoke-Expression`
`kubectl apply -f manifests/deployment.yaml`
`kubectl apply -f manifests/service.yaml`
Access the Application:
Get the minikube IP: `minikube ip`
And copy it to your browser.
Clean up :
`minikube delete`
-----------------------------------
# Follow-up Questions:

## Kubernetes Setup:
I currently have no access to any cloud provider so for this setup I used a local minikube setup for development. 
For production, I think that opting for a managed Kubernetes service like GKE, EKS, or AKS is ideal as it offers easy scalability, high availability, and managed updates.
For the next cluster, the setup would still depend on my accessibility to a cloud provider, as I might consider a managed Kubernetes setup in the cloud for scalability and management benefits.

## Improving Application Setup:

a. I would certainly opt to improve our setup through a CI/CD pipeline to automate the build and deploy processes. 
We have different tools like Jenkins, GitLab CI, or GitHub Actions.. The choice will depend on many factors but personally I would go for Gitlab CI or GitHub Actions, depending on which platform we are using, as they are natively integrated to their respective platforms so no complexities and we will be using one platform to manage everything.
b. Templating the setup can be highly beneficial, especially in we are anticipate reusability, need consistency, and want to automate our CI/CD or infrastructure setup.
Helm is a great choice for templating Kubernetes deployments as it provides package management features, it wasn't used for our current setup but would be added if we envision reusability.
c. Monitoring is crucial for ensuring the health and performance of our application. 
Prometheus for metrics and Grafana for visualization would be a great combo of tools . We may consider monitoring :
- Resource Utilization(CPU/ Memory usage)
- Kubernetes-Specific Metrics( Pod and Node Status)
- Application Health (Error Rate)
Also, we may consider keeping an eye on our logs and opt for different logging solutions such as Elasticsearch.
## Scaling Across Regions:
To do so we may :
- Use a global load balancer that routes traffic to the nearest deployment based on latency.
- Set up Kubernetes clusters in different regions and deploy the application to all of them.
- Use a global database solution or replicate the database across regions, ensuring data consistency.

------------------------------------------
# Getting started

To get the Node server running locally:

- Clone this repo
- `npm install` to install all required dependencies
- Install MongoDB Community Edition ([instructions](https://docs.mongodb.com/manual/installation/#tutorials)) and run it by executing `mongod`
- `npm run dev` to start the local server

Alternately, to quickly try out this repo in the cloud, you can [![Remix on Glitch](https://cdn.glitch.com/2703baf2-b643-4da7-ab91-7ee2a2d00b5b%2Fremix-button.svg)](https://glitch.com/edit/#!/remix/realworld)

# Code Overview

## Dependencies

- [expressjs](https://github.com/expressjs/express) - The server for handling and routing HTTP requests
- [express-jwt](https://github.com/auth0/express-jwt) - Middleware for validating JWTs for authentication
- [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken) - For generating JWTs used by authentication
- [mongoose](https://github.com/Automattic/mongoose) - For modeling and mapping MongoDB data to javascript 
- [mongoose-unique-validator](https://github.com/blakehaswell/mongoose-unique-validator) - For handling unique validation errors in Mongoose. Mongoose only handles validation at the document level, so a unique index across a collection will throw an exception at the driver level. The `mongoose-unique-validator` plugin helps us by formatting the error like a normal mongoose `ValidationError`.
- [passport](https://github.com/jaredhanson/passport) - For handling user authentication
- [slug](https://github.com/dodo/node-slug) - For encoding titles into a URL-friendly format

## Application Structure

- `app.js` - The entry point to our application. This file defines our express server and connects it to MongoDB using mongoose. It also requires the routes and models we'll be using in the application.
- `config/` - This folder contains configuration for passport as well as a central location for configuration/environment variables.
- `routes/` - This folder contains the route definitions for our API.
- `models/` - This folder contains the schema definitions for our Mongoose models.

## Error Handling

In `routes/api/index.js`, we define a error-handling middleware for handling Mongoose's `ValidationError`. This middleware will respond with a 422 status code and format the response to have [error messages the clients can understand](https://github.com/gothinkster/realworld/blob/master/API.md#errors-and-status-codes)

## Authentication

Requests are authenticated using the `Authorization` header with a valid JWT. We define two express middlewares in `routes/auth.js` that can be used to authenticate requests. The `required` middleware configures the `express-jwt` middleware using our application's secret and will return a 401 status code if the request cannot be authenticated. The payload of the JWT can then be accessed from `req.payload` in the endpoint. The `optional` middleware configures the `express-jwt` in the same way as `required`, but will *not* return a 401 status code if the request cannot be authenticated.


<br />

[![Brought to you by Thinkster](https://raw.githubusercontent.com/gothinkster/realworld/master/media/end.png)](https://thinkster.io)
