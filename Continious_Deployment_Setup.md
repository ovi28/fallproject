## Live Application

[frontend](http://ec2-52-213-34-107.eu-west-1.compute.amazonaws.com/)

[backend](http://ec2-52-213-34-107.eu-west-1.compute.amazonaws.com:3000/)

[Jenkins (not freely accessible / requires login)](http://ec2-52-213-34-107.eu-west-1.compute.amazonaws.com:8080/)


## Technology Choice

Jenkins, Docker, AWS EC2, Ubuntu Linux Server

## Steps

1) Launched an EC2 instance (ami-785db401) in Amazon Web Services

2) Installed Jenkins and Docker on it via ssh

```
ssh -i <private_key_file>.pem ubuntu@<public_dns>
# jenkins install commands
# docker install commands
```

3) Configured security group in AWS to open inbound connections to ports that will be used later (8080 for Jenkins, 3000 for backend server and default http 80 for frontend app)

![security_groups](https://preview.ibb.co/dXY665/security_group.png)

4) Secured Jenkins, installed bitbucket plugin with its dependencies and created jobs for the two sub-systems by setting up a webhhok in their bitbucket repos

5) Created Dockerfiles for the two respective projects

![back_end_dockerfile](https://image.ibb.co/gKXYm5/dockerfile_backend.png) ![front_end_dockerfile](https://image.ibb.co/mpJCeQ/dockerfile_frontend.png)

6) Allowed Jenkins to execute sudo commands by adding ``` jenkins ALL=(ALL) NOPASSWD: ALL ``` to ``` /etc/sudoers ``` file

7) Set up following shell script to be run on push to the repo, making sure that on every deploy, old containers are removed

```
NAME=backend
sudo docker build -t $NAME .
sudo docker stop $NAME
sudo docker rm $NAME
sudo docker run -p 3000:8083 -d --restart=always --name $NAME $NAME
```

```
NAME=frontend
sudo docker build -t $NAME .
sudo docker stop $NAME
sudo docker rm $NAME
sudo docker run -p 80:5000 -d --restart=always --name $NAME $NAME
```

8) Validation

![docker_ps](https://preview.ibb.co/bUSzR5/docker_ps.png)
