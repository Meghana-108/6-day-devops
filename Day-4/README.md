- 01/04/2025  
- devops engineers  
- tester  
- use repos from both engineers and try to deploy  
- first deploy in docker then it goes to production stage  
- containerization tool we use is docker  
- all the files are stored as a single unit or file/repo  
- docker is a containerization software; it will make the application deployment process easy and simple  
- the application will be executed as a container (it includes all dependencies, libraries, etc.)  

 Docker Architecture:  
1) Dockerfile contains set of instructions for building an image  
2) Docker image build it ..it contains source code and lib  
3) docker registry to store it  
4) container is made  

-------------------  

 Launch instance in Ubuntu:  
- sudo  
- sudo su  
- apt update -y  
- dockerdocs -> docker install command  
- docker --version  
- touch index.html vi index.html  
- ls right-click paste the code  
- esc :wq enter  
- cat index.html  
- touch Dockerfile (syntax Cap D)  
- ls  
- vi Dockerfile/nano Dockerfile  

------------------------------------------  

 Dockerfile:  
 
FROM ubuntu:latest  
WORKDIR /COLOR  
RUN apt-get update -y  
RUN apt-get install apache2 -y  
copy . /var/www/html  
EXPOSE 80  
CMD ['apachect1', '-D', 'FOREGROUND']  

-------------------------------------  

- Esc :wq / esc :wq!  
- cat Dockerfile  
- docker image build -t red .  
- docker images  
- docker run -d --name blue -p 80:80 red  
- (create and start a container) (detached mode for deploying in an environment) (host port number:container number)  
- docker ps  
- (to see running containers)  
- docker ps -a  
- (to find all containers)  
- docker logs red  
- (to check where there is an error) (mention container name or id)  
- service apache2 status  
- (there is an error) (update the Dockerfile)  

- ls (to edit) (remove quotation and square bracket)  
- esc :wq enter  
- cat Dockerfile  
- docker image build -t green .  
- (update Dockerfile) (ENTRYPOINT ['apachect1', '-D', 'FOREGROUND'])  
- docker image build -t green-v2 .  
- (insert double quotes and update the Dockerfile) (ENTRYPOINT ["apachect1", "-D", "FOREGROUND"])  
- docker image build -t green-v3 .  
- docker run -d --name yellow -p 81:80 green-v3  
- docker ps (to check for running containers)  
- docker inspect yellow  
- (we can give container name/container id)  
- curl 172.17.0.2:80  
- (IP address) (container port number) (we get application source code)  

 Instance Configuration:  
- instance id -> security -> security groups link -> edit inbound rules -> add rule -> all traffic 0.0.0/0/0 -> save rules  
- public IPv4 address:port  
- http://3.39.226.214:81/  

-----------------------------------------------  

 Docker Hub:  
- dockerhub.com  
- signup  
- personal  
- Gmail ID  
- username  

-------------------  

 If you refresh the page:  
- run the command: sudo su  

------------------  

 Tag and Push to Docker Hub:  
- docker tag green-v2:latest meghana108/green-v2:latest  
- docker login  
- (copy the URL and paste the code and confirm)  
- docker push meghana108/green-v2:latest  
- go to Docker Hub and check in my profile ..the containers would have been pushed ..similarly push all the containers  
- docker tag red:latest meghana108/red:latest  
- docker push meghana108/red:latest  

-----------------------------------------------------------  

 Docker Image Management:  
- docker images  
- docker rmi red (to delete)  
- docker images  
- docker pull meghana108/red:latest (from Docker Hub)  
- docker images  

 Container Management:  
- docker pause black  
- docker ps  
- docker unpause black  
- docker ps  
- docker stop black  
- docker restart black  
- docker rm black  
- docker rm $(docker ps -aq) (to delete all containers after rm -f for all)  

------------------  

 Pulling Common Docker Images:  
- docker pull ubuntu:latest  
- docker pull node:latest  
- docker pull mysql:latest  
- docker pull nginx:latest  
- docker pull python:latest  

------------------  

 GitHub Repository and Running Docker Image:  
- GitHub Davidgit526  
- docker run -d --name ipl-v2 -p 86:80 daviddocker526/ipl  
- http://3.39.226.214:86/  

----------------------  

 Executing Commands Inside a Running Container:  
- docker exec -it ipl-v2 bash  
- ls  
- cd /var/www/html  
- ls  
