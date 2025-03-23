# Docker
### simple command line for Docker
1. pull a image
   > docker pull <image-name>
   
   > docker pull hsinyi/hello-world

   pull the image from hsinyi's docker hub
   
3. check images
   > docker images

4. check container 
   > docker container ls

5. run images
   > docker container run images:tag

6. run images from Dockerfile. Go to the Dockerfile folder and run the build or provide the abs-path to build
   > docker build -t <the_image_name_to_be_built> .
   
   > docker build -t hsinyi/hello-world .

7. clean images
   > docker rmi <image-name>
   
   need to ensure that no container is referred, otherwise need to rm the container first
   
   > docker rm -f <image-name>
   
   force to delete the image
  
8. run container: 當服務不被繼續處理，則container就會消失
    > docker container run --name <container-name> <image-name> __ls__
    
    在某個container下啟動image跑ls這個指令，同等於:
   
    > docker run --name <container-name> <image-name> ls
    
    > docker run *-it* --name <container-name> <image-name> */bin/sh*
    
    進入container環境, -it為interactive mode

10. run container: 讓container持續跑某個指令
    > docker container run *-d* --name <continer-name> <image-name> __tail -f /dev/null__
    
    -d為deamon樣式，container會印出log

 11. 進入container
     > docker exec *-it* <container-id> */bin/bash* or *-- bash*

12. using ngix to create a container
    > docker pull ngix
    
    > docker run -d -p 8081:80 --name <image-name> ngix
    
    container port 8081 and ngix port 80
    
13.  check docker VM Linux IP
    > echo $(docker-machine ip)

14. stop container
    > docker container ls -a
    
    確認啟動中以及有啟動過的所有container
    
    > docker container stop <container-id>

## Create Jenkins on Docker
1. start Jenkins on Docker [Jenkins|https://github.com/jenkinsci/docker/blob/master/README.md]
    > docker run -d -v jenkins_home:/var/jenkins_home -p 8080:8080 -p 50000:50000 --restart=on-failure jenkins/jenkins:lts-jdk17
    
    -d(deamon) -v(volumn) 都是確保在container關閉後服務都還存在

   _web port: 8080, ssh port:50000_
   
3. check volumn
   > docker volumn ls

4.  get the first login password on Jenkins
   > docker logs <container-id>
   
   ![image](https://github.com/user-attachments/assets/0085a676-8b9e-44aa-8983-108b71e782f6)

## create Dockerfile


   
