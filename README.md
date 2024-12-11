# helloworldjavaJava Web Application: Hello World
This README provides a step-by-step guide to setting up a simple "Hello World" Java web application, containerizing it with Docker, and deploying it on Kubernetes.

Prerequisites
Ensure you have the following installed:

[Java JDK 8+]
[Maven]
[Docker]
[Kubernetes]
[kubectl CLI]
1. Creating the Java Web Application
Step 1: Update the Web Application
Modify the src/main/webapp/index.jsp file to display "Hello World":

<html>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
Add a Dockerfile in the root of your project:

FROM tomcat:9.0
COPY target/hello-world.war /usr/local/tomcat/webapps/
Package the application as a WAR file:

mvn package
2. Containerizing with Docker
Build the Docker image:

docker build -t hello-world-app .
Run the Docker container:

docker run -d -p 8080:8080 hello-world-app
Verify the application by visiting http://localhost:8080/hello-world.

3. Deploying on Kubernetes
Step 1: Create Kubernetes Deployment and Service
Create a deployment.yaml file:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mydeploy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: satyajitpani97/myimg
        image: myhelloworldimg
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: my-svc
spec:
  selector:
    app: myy-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
    nodeport:32000
    type: NodePort
Apply the configuration:

kubectl apply -f deployment.yaml
Step 2: Access the Application
Get the NodePort of the service:

kubectl get svc my-service
Access the application using the external IP and NodePort:

http://<EXTERNAL-IP>:<NODE-PORT>/hello-world
Cleanup
To remove the resources:

Stop and remove the Docker container:

docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>
Delete the Kubernetes deployment and service:

kubectl delete -f -deployment.yaml
Notes
Ensure your Kubernetes cluster is running and configured correctly before deploying.
Update image in the Kubernetes manifest file with the appropriate repository and tag if pushing to a container registry.