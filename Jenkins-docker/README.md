# Jenkins-Docker deployment project

Here we create docker image and deploy it on a remote machine using jenkins as a deployment tool.

## Requirements:

1. git repository with code.
2. A jenkins machine
3. A remote docker machine.


## 1. Configure Jenkins server.

1) I used a azure VM with ubuntu OS for this purpose.
Configure inbound rules to accept traffic on ports 22, 8080 and 80. 

2) Login to the machine with the private key on putty.

3) Install Jenkins 
    
        wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
        sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
        sudo apt update
        sudo apt install jenkins

__Problems faced:__

   1) During apt update:

             Reading package lists... Done
            W: GPG error: https://pkg.jenkins.io/   debian-stable binary/ Release: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 5BA31D57EF5975CA
            E: The repository 'http://pkg.jenkins.io/debian-stable binary/ Release' is not signed.

 _Solution:_

            sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA



4) Access http://<ipadress:8080> on the browser and follow along the steps.
5) Jenkins is ready.

## 2. Install docker on remote machine. 

1) I used a azure VM with ubuntu OS for this purpose.
Configure inbound rules to accept traffic on ports 22, 8080 and 80. 

2) Login to the machine with the private key on putty.

3) Install docker 

    1.Set up  Docker's apt repository.

        # Add Docker's official GPG key:
        
        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg
        sudo install -m 0755 -d /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        sudo chmod a+r /etc/apt/keyrings/docker.gpg

        # Add the repository to Apt sources:
        echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
        sudo apt-get update

    2.To install latest version of docker

        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

4) Create a new user dockeradmin

        sudo useradd dockeradmin -m
        sudo usermod -aG docker dockeradmin


## 3. Enable SSH login between jenkins and docker machine.

  1. Generate SSH keys on jenkins machine.

            azureuser@Jenkins:~$ ssh-keygen
  2. Generate SSH keys on docker machine

            azureuser@docker:~$ sudo su - dockeradmin
            dockeradmin@docker:~$ ssh-keygen
  3. Copy jenkins machine pub key to docker machine authorized_keys and vice versa.


## 4. Get github repository ready.

https://github.com/anuja2015/DevOpsPractice.git

## 5. Install SSH, Publish over SSH, Docker, GitHub Integration, Maven Integration plugins on Jenkins

__Manage Jenkins-> Plugins-> Available Plugins__.

## 6. Add SSH server on Jenkins.

__Manage Jenkins-> System -> Add SSH server

<img width="800" alt="ssh" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/c93c7e54-99be-48a6-918f-38525e02035b">


Add the private key of the jenkins machine in the advanced settings.

<img width="859" alt="sshadvanced" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/7529156f-1fe5-4443-b9d5-63295096eef2">

        
## 7. Create a dockerfile 

This  is a simple httpd application to be dockerized. Let's call it finexoapp. The files including index.html are present inside finexo directory within Jenkins-docker folder in the repository. 
        
        
          FROM httpd:latest
          COPY ./finexo/ /usr/local/apache2/htdocs/

Push it to the repository.

## 8. Create a jenkins freestyle job and Configure

<img width="654" alt="sourcecodemanagement" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/821eb715-1758-4ca9-91e2-c2c83d9dfa14">


<img width="415" alt="buildenvironment" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/906ef3a2-fd96-42bc-9f70-91b066cd78d1">


<img width="634" alt="Buildstep1" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/d5a5f5bc-6d32-4d1f-b901-87753f4003fa">


<img width="629" alt="Buildstep2" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/d5b4ff5b-2530-47b4-8087-e9de501ad8b8">


## 9. Build now.






