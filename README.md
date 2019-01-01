

# About ASG2
This simple DevOps task. Below are the instructions to run the task.

Docker image is created and pushed to dockerhub 

    docker.io/shahabshahab2/shahabshahab2/backbase_tomcat:8
    docker.io/shahabshahab2/shahabshahab2/backbase_nginx:1
    docker.io/shahabshahab2/shahabshahab2/backbase_jenkins:2.7.4

Dockerfile code for each module is in it's relative folder.

Kubernetes code is in deployment folder. Kubernetes deployment has been created with replica set of 3 for nginx andnreplica set of 1 for tomcat(v8) and replica set of 1 for Jenkins . In services app has been exposed with Nodeport to access from outside the cluster. To get the service first get the minikube IP and then run it with port exposed i.e. 8080.

Technical Task Details are as below:
- Based on minikube
- Deploy 3 replicas of nginx/apache
- Include a simple "hello world" in the index.html of nginx/apache
- Deploy 1 replica of tomcat v8
- Deploy sample.war to tomcat deployment (use this: https://tomcat.apache.org/tomcat-8.0-doc/appdev/sample/ )
- Deploy a jenkins pod (1 replica)
- Configure a static "dummy" job in jenkins

# Project Arch 
The project architecture is as follow: 

![][Arch]




1) Docker Image. 

   #To build the docker image from dockerfile 
   
   a) cd nginx

   b) docker build  shahabshahab2/backbase_nginx:1 .
  
   #To run the created docker image 
   
   b) docker run -it -p 8080:8080 shahabshahab2/backbase_nginx:1

   #To push the image from docker hub 
   
   c) docker push shahabshahab2/backbase_nginx:1

the same for tomcat and Jenkins

2) Kubernetes
   
   #To deploy   
   
   a) kubectl apply -f deployment/

   #To get the deployments of app  
   
   b) kubectl get deployments -n backbase -o wide
 
   #To get the pods of app 
   
   c) kubectl get po -n backbase -o wide
   
   #To get the services of app 
   
   d) kubectl get svc -n backbase -o wide
      minikube service -n backbase list

Some notes about Jenkins :

   #To get the service url
   
   e) minikube service -n backbase jenkins-expose-service —-url
   
   # User and pass for jenkins 

   f) The admin user and password is set to admin/admin by default-user.groovy in creating the docker image.

   #Jenkins plugins
   
   g) The setup wizard skipped by setting -Djenkins.install.runSetupWizard=false and some instance plugins loaded by \
     importing plugins.txt into install-plugins.sh script.
   h) The dummy job is imported while creating the image in the Docker file. As Jenkins is running by Jenkins user, ownership of the \
      dummy jobs should changed to Jenkins as it is getting copy ( --chown=jenkins:jenkins )


To run the above app. 

1) First clone the repository. 
2) Start the minikube cluster with command minikube start.
3) Now for kubernetes first create namespace and the deployment and services. 
  
   a) kubectl apply -f deployment/namespace.yaml &&  kubectl apply -f deployment/
   Verify if namespace with name backbase is created by running kubectl get ns. 
   Verify if deployment has been created by running kubectl get deploy -n backbase and has appropriate "Available" value.
   Verify if service has been created by running kubectl get svc -n backbase.
 
   b) Now run your service. First check IP of your minikube (command : minikube ip) as we have used nodeport the service will run on IP of minikube 
   with port we forward in service i.e. 
      
     $ minikube service jenkins-expose-service -n backbase --url
      http://192.168.99.100:32752

   c) One other way to view deployed service is to expose them manually i.e.
      
      for nginx:

     $ kubectl expose deploy/nginx-deployment --type=NodePort --target-port=8080 -n backbase --name  "nginx-expose-service"
     $ minikube service nginx-expose-service -n backbase --url 
      http://192.168.99.100:30859

      for tomcat:
      
     $ kubectl expose deploy/tomcat-deployment --type=NodePort --target-port=8090 -n backbase --name    "tomcat-expose-service"
     $ minikube service tomcat-expose-service -n backbase --url
       http://192.168.99.100:31449


   
[Arch]: https://cdn1.imggmi.com/uploads/2018/12/31/b3e2228850aab76caec334db03ba4666-full.png
