### create-simple-devops-image.yml
### you must be loged in to dockerhub as root if you must use "become: true" else ensure that the account you are using is logged into docker hub ### 
### The .Dockerfile is in the /opt/docker directory and instructs the war file to be coppied to the /webapps directory in the container image to be created COPY ./webapp.war /usr/local/tomcat/webapps

---
- hosts: all
  become: true

  tasks:
  - name: create docker image using war file
    command: docker build -t simple-devops-image:latest .
    args:
      chdir: /opt/docker

  - name: create tag to image
    command: docker tag simple-devops-image ehonor/simple-devops-image


  - name: push image on to dockerhub
    command: docker push ehonor/simple-devops-image
    ignore_errors: yes

  - name: remove docker images from ansible server
    command: docker rmi simple-devops-image:latest ehonor/simple-devops-image
    ignore_errors: yes

### create-simple-devops-project.yml
### become true is commented out because you won't always be root on all the servers you are deploying to

- hosts: all
#  become: true

  tasks:

  - name: stop current running container
    command: docker stop simple-devops-container
    ignore_errors: yes

  - name: remove stopped container
    command: docker rm simple-devops-container
    ignore_errors: yes

  - name: remove docker image
    command: docker rmi ehonor/simple-devops-image:latest
    ignore_errors: yes

#  - name: build docker image war file
#    command: docker build -t simple-devops-image .
#    args:
#      chdir: /opt/docker

  - name: pull docker image from dockerhub
    command: docker pull ehonor/simple-devops-image:latest

  - name: create container using simple-devops-image
    command: docker run -d --name simple-devops-container -p 8080:8080 ehonor/simple-devops-image

