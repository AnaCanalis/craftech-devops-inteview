# Craftech Technical Interview
# Steps to dockerize a Django + React.js application 

Instructions to compile and deploy an application on the pc.  
It was compipled and deployend on a Virtual Machine with Ubuntu. 

Note: the docker-compose.yml file is not finished yet. At the moment, backend and frontend can be run separately.

## Requirements

To run this app you need to install Docker and docker-compose. If you have those tools in your machine, please go to the next section.

_Install Docker on Ubuntu_

Config file /lib/systemd/system/docker.service 

Copy and paste the following commands one by one in the Terminal 


* Update the repository
```
sudo apt-get update
```

* Install utilities
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y
```

* Add gpg 
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

* Add the repository
```
 sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

* Update again
```
 sudo apt-get update
```

* Install Docker
```
 sudo apt-get install docker-ce
```

* Inizialize Docker with system
```
sudo systemctl enable docker
```

* Add user to Docker's group
```
whoami # Saber el nombre de tu usuario
sudo usermod -aG docker nombre_de_salida_en_whoami
```

* Sign Off
```
exit
```

* Login again with the user and test 
```
docker run hello-world
```

_Install Docker-compose_

* Change to root user for permissions
```
sudo su
```
* Copy and paste. (For more information you can check this link https://docs.docker.com/compose/install/)
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

* Give execution permissions
```
chmod +x /usr/local/bin/docker-compose 
```

* Test
```
docker-compose
```
## Build and run your app with Docker Compose

You don’t need to install React.js, Node.js, Python or Django all are provided by images. Node and Python by Docker images and React.js and Django are custom-images defined on Dockerfiles. 

* Create a project directory and change 
```
mkdir directory-name
cd directory-name
```

* Clone the repository

```
git clone https://github.com/AnaCanalis/craftech-devops-inteview.git
```
Now, you have all the files that you need to run this application

* From your project directory, start up your application by running 
```
docker-compose up -d --build
```
The docker-compose.yml file has the necessary information to build all the images and containers 

If you want to build the images separately, please go to the next section
  
## Build and run your app with Docker

After creating dockerfile, you only need to run the following commands
```
docker build -t image-name -f Dockerfile-name . 
docker -d -ti -p host:container image-name
```
_For build and run this application it is important that you use the same Dockerfiles names because they are defined in files that you download from the repository. If you want to modify them, remember to changing it when you build the image_

Copy and paste the following commands one by one in the Terminal 

### Frontend
```
docker build -t react-front -f Dockerfile-frontend .
```
```
docker -d -ti -p 3000:3000 react-front
```
Your image is built and running on a container, you can check enter it in your browser https://localhost:3000

### Backend
```
docker build -t django-back -f Dockerfile-backend .
```
```
docker -d -ti -p 8000:8000 react-front
```
Your image is built and running on a container, you can check enter it in your browser https://localhost:8000


