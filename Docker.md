# Docker
### Command for Docker
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

10. run container: 讓container持續在背景模式下執行某個預設指令
    > docker container run *-d* --name <continer-name> <image-name> __tail -f /dev/null__
    
    -d為deamon樣式，container會印出log

11. 進入container
     > docker exec *-it* <container-id> */bin/bash* or *-- bash*

12. using ngix to create a container
    > docker pull ngix
    
    > docker run -d -p 8081:80 --name <image-name> ngix
    
    Linux VM port 8081 and container port 80
    
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

## Dockerfile 

### Docker觀念

1. Dokcer Client(terminal): 執行所有docker的指令
2. Docker Engine: 接收docker client來的指令
3. 使用到的空間有: Physical Host, Linux OS(VM), Temp Container(built by Docker)

![image](https://github.com/user-attachments/assets/f1753419-3b39-4b5c-8800-103a04e1e640)

### 語法

* run Dockerfile: docker build -t <the_image_name_to_be_built> .

1. __FROM__ : 尋找並pull base image的來源
   >FROM apline:latest

2. __ENV__: 環境變數讓執行指令的時候都在同一個環境下
   > ENV <變數名稱> <絕對路徑>

   > ENV workdir /var/www/localhost
3. __ARG__: 在run docker build的時候可以去改變變數
   > ARG <變數名稱>=<預設變數>
   
   > ARG whoami=COOL
   
   若build image的時候沒有指定，則是用預設值
   
   > docker build _--build-arg whoami=haha_ -t hsinyi/001

   用--build-arg來重新指定變數
   
4. __WORKDIR__: 直接將執行目錄到ENV下
   > WORKDIR ${<env_name>}

   > WORKDIR ${workdir}

5. __RUN__: 透過run來執行page image提供的指令
   > RUN apk --update add apache2

   在跑apline container下安裝apache2
   
   > RUN rm -rf /var/cache/apk/*

   安裝完後rm cache

   > RUN cd /var/log && ls
   
   每一次RUN都會起一個新的Container，所以前後RUN並不會有關聯，除非用 && 關聯兩個指令
   
   > RUN ${workdir} \ && echo "cool"
   
   > RUN ${workdir} \ && echo "cool2"
   
   > RUN ${workdir} \ && echo "cool3"

   用環境變數來確保在同一個path下執行

   > RUN echo "cool"
   
   > RUN echo "cool2"
   
   > RUN echo "cool3"

   若加入WORKDIR的話，就已經有設定工作目錄，所以就可以不用再設定ENV

   > RUN echo "${whoami}"
   
   > RUN echo "${whoami}2"
   
   > RUN echo "${whoami}3"

   若加入WORKDIR的話，就已經有設定工作目錄，所以就可以不用再設定ENV

6. __COPY__: 當檔案放在當Container的目錄下
   > COPY ./mock.html /.
   
7. __ENTRYPOINT__: docker run某個image後，image會第一個執行的指令
   > ENTRYPOINT ["httpd","-D","FOREGROUND"]

   啟動WEB SERVER

### container 轉 image再轉container的語法 {docker commit}

1. docker commit <container-id> <new_image_name>
   > docker commit [container-id] hsinyi/containertoimage

2. docker run -d -p ports:ports <new_image_id>


   
