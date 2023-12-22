
# Jenkins-Ansible-Docker

Here we create docker image and push it to dockerhub using ansible on Jenkins. We then deploy the container of the same image on a remote machine using ansible and jenkins.

## Requirements:

1. git repository with code.
2. A jenkins machine
3. Ansible machine
4. A remote docker machine.

We install Jenkins and ansible server on the same machine.


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

## 2. Install Ansible

        azureuser@Jenkins:~$ sudo apt-add-repository ppa:ansible/ansible
        sudo apt update
        sudo apt install ansible

## 3. Install docker on remote machine. 

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
        sudo vi /etc/sudoers
        # User privilege specification
        root    ALL=(ALL:ALL) ALL
        dockeradmin  ALL=(ALL:ALL) ALL


## 4. Enable SSH login between jenkins user on jenkins and dockeradmin on docker machine 

  1. Generate SSH keys on jenkins machine.

            azureuser@Jenkins:~$ ssh-keygen
  2. Generate SSH keys on docker machine

            azureuser@docker:~$ sudo su - dockeradmin
            dockeradmin@docker:~$ ssh-keygen
  3. Copy jenkins machine pub key to docker machine authorized_keys and vice versa.

## 5. Get github repository ready.

https://github.com/anuja2015/DevOpsPractice.git

## 6. Install SSH, Publish over SSH, Docker, GitHub Integration, Maven Integration plugins on Jenkins

__Manage Jenkins-> Plugins-> Available Plugins__.

## 7. Add SSH server on Jenkins.

__Manage Jenkins-> System -> Add SSH server

<img width="800" alt="ssh" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/c93c7e54-99be-48a6-918f-38525e02035b">


Add the private key of the jenkins user on jenkins machine in the advanced settings.

<img width="859" alt="sshadvanced" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/0765d627-dbd3-4bbd-9ba9-c02415bd22a8">

## 8. Create a new user jenkins on docker machine
        sudo useradd jenkins -m
        # create a new password for jenkins user
        sudo passwd jenkins
         # Add jenkins to sudoers.
         # User privilege specification
        root    ALL=(ALL:ALL) ALL
        jenkins  ALL=(ALL:ALL) ALL
        sudo su - jenkins
        sudo su - dockeradmin
        [sudo] password for jenkins:
        #password will be asked once.
        
## 9. Create a jenkins freestyle job and Configure

<img width="654" alt="sourcecodemanagement" src="https://github.com/anuja2015/DevOpsPractice/assets/16287330/821eb715-1758-4ca9-91e2-c2c83d9dfa14">







