# Microservices-Deployment
This repository contains a sample application that has multiple microservices that I've Dockerized and deployed on a Kubernetes cluster.



### Credits

Special thanks to **Deekshith** for contributing to this project. **Deekshith** wrote the Code for the microservices and creating Application Architecture Design. 
Thank you for your hard work and dedication to this project.


## Application Architecture

#### In the UI, as the user clicks on a specific button, specific data is sent to API-Gateway, and depending on that, it will validate the request and redirect to a specific microservice (respective API).
#### The application consists of 6 different Microservices. Architecture of sample multiple microservices developed in different technologies - Spring Boot, Node.js, Python, React.js in a project. Microservices connected by an API Gateway using Netflix Zuul. 




##### Frontend - UI
-   [ui-web-app-reactjs](https://github.com/sarat9/microservices-architect-config-starter/tree/main/ui-web-app-reactjs)  - Single Page Application that provides the UI
- Click (ui-web-app-reactjs) 
- Click (src) 
- Click (Components) 
- Click (App.jsx)
###### This is where you need to provide details about api-gateway. 
     <button name="shoe/shoes" onClick={handleApiCall} style={{margin:'0px 10px'}}>Shoes</button>
     <button name="offer/offers" onClick={handleApiCall} style={{margin:'0px 10px'}}>Offers</button>
     <button name="cart" onClick={handleApiCall} style={{margin:'0px 10px'}}>Cart</button>
     <button name="wishlist" onClick={handleApiCall} style={{margin:'0px 10px'}}>Wishlist</button>



#####  APIGateway -  Zuul 
-   [zuul-api-gateway](https://github.com/sarat9/microservices-architect-config-starter/tree/main/zuul-api-gateway)  - API gateway that proxies all the micro-services

- Click (Zuul-api-gateway)
- Click (src)
- Click (main)
- Click (resources) 
- Click (application.yml) 

##### This is where you need to configure if my suffix is offer, shoe, wishlist, or cart to go to this particular api. Depending on whatever the URL that i recieve it from UI defining what is the destination to reach.

```
    offer:
      path: /offer/**
      url: http://100.25.193.105:1001/api/v1
    shoe:
      path: /shoe/**
      url: http://100.25.193.105:1002/api/v1
    wishlist:
      path: /wishlist/**
      url: http://100.25.193.105:1003
    cart:
      path: /cart/**
      url: http://100.25.193.105:1004/api/v1
```



##### Backend Services - Cart, Offers, Shoes, Wishlist 
-   [cart-microservice-nodejs](https://github.com/sarat9/microservices-architect-config-starter/tree/main/cart-microservice-nodejs)  - Node.js App with Cart data
-   [offers-microservice-spring-boot](https://github.com/sarat9/microservices-architect-config-starter/tree/main/offers-microservice-spring-boot)  - Spring Boot App with Offers data
-   [shoes-microservice-spring-boot](https://github.com/sarat9/microservices-architect-config-starter/tree/main/shoes-microservice-spring-boot)  - Spring Boot App with Shoe data
-   [wishlist-microservice-python](https://github.com/sarat9/microservices-architect-config-starter/tree/main/wishlist-microservice-python)  - Python App with Wishlist data

![MicroService Architeture ](https://miro.medium.com/max/1050/1*kSLJKEl3X-gKNTpO1l7SQg.png)

### Tools Covered
- Docker 
- Kubernetes 

### Step by Step to Build and Deploy Microservices & Bring Up the Application 

- Step 1 - Create an instance named Build-Server and login to the Build-Server. Install Git & Docker.
- Step 2 - Update your Build-Server public ip in ui-web-app-reactjs, zuul-api-gateway in GitHub 
- Step 3 - Clone the Microservices-Deployment repository.
- Step 4 - Create a Dockerfile for each microservice, build the Dockerfile, and run it as a container.
- Step 5 - Push images to Docker Hub
#### Step 1 - Create an instance named Build-Server and login to the Build-Server. Install Git & Docker.

- Become root user
```
sudo su - 
```
##### Install Git
```
yum install git -y
```
##### Install Docker 
```
yum install docker -y
``` 
```
service docker start
```
```
service docker status
```
- To confirm weather docker service is running or not 
```
docker info  
```

#### Step 2 - Fork the repository. Update your Build-Server public ip in ui-web-app-reactjs, zuul-api-gateway in your GitHub.   

###### ui-web-app-reactjs
- Click (ui-web-app-reactjs) 
- Click (src) 
- Click (Components) 
- Click (App.jsx)


```
const url = 'http://100.25.193.105:9999/'+e.target.name;   # http://give your Build-Serverip:9999/
```

###### zuul-api-gateway

- Click (Zuul-api-gateway)
- Click (src)
- Click (main)
- Click (resources) 
- Click (application.yml) 

```
server:
  port : 9999
zuul:
  routes:
    offer:
      path: /offer/**
      url: http://100.25.193.105:1001/api/v1   # http://your Build-Serverip:1001/api/v1
    shoe:
      path: /shoe/**
      url: http://100.25.193.105:1002/api/v1   # http://your Build-Serverip:1001/ap2/v1
    wishlist:
      path: /wishlist/**
      url: http://100.25.193.105:1003          # http://your Build-Server ip:1003
    cart:
      path: /cart/**
      url: http://100.25.193.105:1004/api/v1    # http://your Build-Server ip:1004/api/v1
```

#### Points to Remember 
- In UI, we need to give the information about the API server.
- In an API server, we need to give information about API services.


#### Step 3 - Clone the Microservices-Deployment repository.

```
git clone https://github.com/Bathalapalli-SaiRangaPavan/Microservices-Deployment.git   # https://github.com/<giveyourrepositoryname>/Microservices-Deployment.git 
```
- Go inside the cloned repository
```
cd  Microservices-Deployment
```
- List the Files and Directories inside the repository
```
ls 
```
![g](https://user-images.githubusercontent.com/121741348/225528546-5cf8c414-6fe0-4f15-b8ad-5f1917e506ce.png)

#### Step 4 - Create a Dockerfile for each microservice, build the Dockerfile, and run it as a container
##### Step 4.1 - Create a Dockerfile for ui-web-app-reactjs, build the Dockerfile, and run it as a container
```
cd ui-web-app-reactjs
```

- Create a Dockerfile based on the instructions. [ui-web-app-reactjs](https://github.com/sarat9/microservices-architect-config-starter/tree/main/ui-web-app-reactjs)

```
FROM node:8 
WORKDIR /app          
COPY . . 
RUN npm install 
EXPOSE 8080 
CMD ["npm","start"]
```
- Build to create an image
```
docker build -t ui .
```
- Check the Images 
```
docker images
```
- Create a container from the image.
```
docker run -d --name ui-container -p 8080:8080 ui
```
- Check the process status
```
docker ps
```
- Note - Open port 8080 in the Security Group inbound rules.

- Browse - http://yourdockerip:8080/       
##### Access Front End 

![rr](https://user-images.githubusercontent.com/121741348/225595769-fce9b60c-69b5-4240-8582-58b7d7b97986.png)

##### Whenever you click on shoes, offers, cart, or wishlist, you are not able to access the particular microservices. 

##### Step 4.2 -  Create a Dockerfile for zuul-api-gateway, build the Dockerfile, and run it as a container 


```
cd zuul-api-gateway
```
-  Create a Dockerfile based on the instructions. [zuul-api-gateway](https://github.com/sarat9/microservices-architect-config-starter/tree/main/zuul-api-gateway)
```
# multi stage build 
FROM maven as build
WORKDIR /app
COPY . .
RUN mvn clean install

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/zuul-0.0.1-SNAPSHOT.jar .
EXPOSE 9999
CMD ["java","-jar","zuul-0.0.1-SNAPSHOT.jar"]
```
- Note - If you go for multistage, you can reduce the image size.

- Build to create an image
```
docker build -t zuul . 
```
- Check the Images 
```
docker images
```
- Create a container from the image
```
docker run -d --name zuul-container -p 9999:9999 zuul
```
##### To see zuul-api-gateway is working as expected 
```
docker logs zuul-container
```
- It Started in 8.83 seconds

- Note - Open port 9999 in the Security Group inbound rules.

##### Step 4.3 -  Create a Dockerfile for shoes-microservice-spring-boot, build the Dockerfile, and run it as a container 

```
cd ..
```
```
cd shoes-microservice-spring-boot
```
- Create a Dockerfile based on the instructions. [shoes-microservice-spring-boot](https://github.com/sarat9/microservices-architect-config-starter/tree/main/shoes-microservice-spring-boot) 

```
FROM maven as build
WORKDIR /app
COPY . .
RUN mvn install

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/shoes-0.0.1-SNAPSHOT.jar /app
EXPOSE 1002
CMD ["java","-jar","shoes-0.0.1-SNAPSHOT.jar"]
```
 
- Build to create an image
```
docker build -t shoes . 
```
- Check the Images 
```
docker images
```
- Create a container from the image
```
docker run -d --name shoes-container -p 1002:1002 shoes
```
- Note - Open port 1002 in the Security Group inbound rules.
- Click (Shoes) - Will get a response like this {"tommy":"Tommy Hilfiger Shoe","adidas":"Adidas Running Shoe","nikeshoe":"Nike Sports Shoe"}

###### Access Shoes Service
![sh](https://user-images.githubusercontent.com/121741348/225594371-b0f49905-d619-47a7-a14e-0ff471759c22.png)

##### Step 4.4 -  Create a Dockerfile for offers-microservice-spring-boot, build the Dockerfile, and run it as a container
```
cd ..
```
```
cd offers-microservice-spring-boot
```
- Create a Dockerfile based on the instructions. [offers-microservice-spring-boot](https://github.com/sarat9/microservices-architect-config-starter/tree/main/offers-microservice-spring-boot)  

```
FROM maven as build 
WORKDIR /app
COPY . .
RUN mvn install 

FROM openjdk:11.0.10-jre
WORKDIR /app
COPY --from=build /app/target/offers-0.0.1-SNAPSHOT.jar /app
EXPOSE 1001
CMD ["java","-jar","offers-0.0.1-SNAPSHOT.jar"]
```
- Build to create an image
```
docker build -t offers . 
```
- Check the Images 
```
docker images
```
- Create a container from the image
```
docker run -d --name offers-container -p 1001:1001 offers
```
- Note - Open port 1001 in the Security Group inbound rules.
- Click  (Offers) - will get a response like this {"samsung":"Samsung 10% Discount","adidas":"Adidas Shoe 70% Off","nikeshoe":"Nike Sports Shoe 50% off"}
###### Access Offer Service 
![o](https://user-images.githubusercontent.com/121741348/225592437-12e3063b-bcf8-4a8f-b9f1-c02177e96bf2.png)

##### Step 4.5 -  Create a Dockerfile for cart-microservice-nodejs, build the Dockerfile, and run it as a container
```
cd ..
```
```
cd cart-microservice-nodejs
```
- Create a Dockerfile based on the instructions.  [cart-microservice-nodejs](https://github.com/sarat9/microservices-architect-config-starter/tree/main/cart-microservice-nodejs)

- Note - It depends on the version if u go with latest version it might not work as expected 
```
FROM node:14
WORKDIR /app 
COPY . .
RUN npm install
EXPOSE 1004
CMD ["node","index.js"]
```

- Build to create an image
```
docker build -t cart . 
```
- Check the Images 
```
docker images
```
- Create a container from the image
```
docker run -d --name cart-container -p 1001:1001 cart
```
- Note - Open port 1004 in the Security Group inbound rules.

-  Click (Cart) - will get a response like this {"data":[{"itemNo":1,"item":"Nike Shoe"},{"itemNo":2,"item":"Tommy Hilfiger Shirt"},{"itemNo":3,"item":"Calvin Klien Trousers"}]}

###### Access Cart Service 
![c](https://user-images.githubusercontent.com/121741348/225591779-5017e7ec-b3e5-4ea2-a10f-559d1b01434d.png)

##### Step 4.6-  Create a Dockerfile for wishlist-microservice-python, build & run as a container
```
cd ..
```
```
cd wishlist-microservice-python
```

- Create a Dockerfile based on the instructions -   [wishlist-microservice-python](https://github.com/sarat9/microservices-architect-config-starter/tree/main/wishlist-microservice-python) 

```
FROM python:3
COPY . .
RUN pip install flask flask_cors
EXPOSE 1003
CMD ["python","index.py"]
```

- Build to create an image
```
docker build -t wishlist . 
```
- Check the Images 
```
docker images
```
- Create a container from the image
```
docker run -d --name wish-container -p 1003:5000 wishlist
```
- Note - Open port 1003 in the Security Group inbound rules.
- Click (Wishlist) - Will get a response like this {"1": "Apple Iphone", "2": "MacBook", "3": "Your Fav Something else"}
###### Access Wishlist Service 
![wi](https://user-images.githubusercontent.com/121741348/225591435-f5f8a8fa-ffb8-414c-ad6a-6d148b469bd1.png)

```
git status 
```
```
git add .
```
```
git commit -m "created Dockerfile for microservices"
```
```
git push origin main
```

- Give Github Credentials
- Check in Github can able to see Dockerfile

#### Step 5 - Push images to Docker Hub

###### PreRequiste - You need to have Docker Hub account 


```
docker login 
```
###### Give your Dockerhub Username & Password
###### Once you login Create a Repository in DockerHub 

- Create a repository named ui

```
docker tag ui bsairangapavan/ui:v1
```
```
docker push bsairangapavan/ui:v1
```
###### You can able to see like below 
![viui](https://user-images.githubusercontent.com/121741348/225616962-cccc0b5f-7353-4577-bf52-6386a0238e62.png)

- Create a repository named zuul 
```
docker tag zuul  bsairangapavan/zuul:v1
```
```
docker push bsairangapavan/zuul:v1
```
###### You can able to see like below 
![zuul](https://user-images.githubusercontent.com/121741348/225617583-f70ae645-7423-479f-898b-f101a80f1d5a.png)

- Create a repository named shoes
```
docker tag shoes bsairangapavan/shoes:v1
```
```
docker push bsairangapavan/shoes:v1
```
###### You can able to see like below 
![shoes1](https://user-images.githubusercontent.com/121741348/225618003-372ded62-233a-4da0-aeb2-d07a3df3c5f6.png)

- Create a repository named offers

```
docker tag offers bsairangapavan/offers:v1
```
```
docker push bsairangapavan/offers:v1
```
###### You can able to see like below 
![offer](https://user-images.githubusercontent.com/121741348/225618486-6a407c0e-ccb2-4e48-bdd5-fa82528d9f39.png)

- Create a repository named cart

```
docker tag cart bsairangapavan/cart:v1
```
```
docker push bsairangapavan/cart:v1
```
###### You can able to see like below 
![d](https://user-images.githubusercontent.com/121741348/225618932-536692e4-d116-4437-8632-e616f712bacc.png)

- Create a repository named wishlist 
```
docker tag cart bsairangapavan/wishlist:v1
```
```
docker push bsairangapavan/wishlist:v1
```
###### You can able to see like below 


![wis](https://user-images.githubusercontent.com/121741348/225619435-6b4cbb74-3ae6-4361-b5f6-cdc3a468078a.png)

