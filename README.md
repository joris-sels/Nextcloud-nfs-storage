
# Welcome to my Docker-Nextcloud-NFS-Storage wiki! 👋

### Before we start I want to say that I have absolutely nothing to do with the Nextcloud team or devs. Netxcloud is in no-way what so ever developed by me. This tutorial just shows you how to link and NFS-share to your Nextcloud docker container. All the credits go to the Nextcloud team for the platform itself!

##  What do you need? 

- In my example I am using 2 different VPS's. 1 with a terrabyte of storage ( Block storage VPS ) and one 
regular front-end VPS. This could also be 2 VM's or just 2 different servers.

- A linux operating system installed ( I use ubuntu server ) on both servers 

- Docker and docker-composed installed.

- Install NFS dependencies.

## Steps to install Nextcloud with external NFS storage.
on the Storage VPS you must install all the necesary stuff to run a NFS server.

If you do not know how to do this I will refer you to the following guide: 

⚠️ **these steps are necessary** ⚠️

[Click here for Ubuntu NFS server and client setup](https://www.tecmint.com/install-nfs-server-on-ubuntu/)

If you followed all the steps correctly you should now have a working NFS share that starts up with you storage server and one server that automatically connects to the NFS share when the system is booted. 


## Docker installation 🐳

🐋 The next step is to install Docker and Docker-compose. 🐋

` $ sudo apt-get update `

` $ sudo apt upgrade`

` $ sudo apt install docker.io`

If you think that went succesfull you can test it by using the following "test container" 

`$ docker run hello-world`

Now we install docker-compose:

` $ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

` $ sudo chmod +x /usr/local/bin/docker-compose`

Now check if the installation was succesfull: 

` docker-compose --version`


### Portainer install (opcional)
Additionally you can install **Portainer**  to easily manage your containers and stacks.                                                           
                                                                                                                                                   
`$ docker volume create portainer_data`                                                                                                            
                                                                                                                                                   
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]                                                                                                      
                                                                                                                                                   
`$ docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data  
portainer/portainer-ce`                                                                                                                            
                                                                                                                                                   
If you now surf on your browser to https://IP-of-your-server:9000  you can find Portainer.                                                       
                                                                                                                                                   

***

Now it is time to start deploying our Nextcloud container. 

⚠️ **what I do next is personal preference you can name the folder whatever you want** ⚠️

`$ sudo mkdir docker-configs`

`$ cd docker-configs `

`$ sudo mkdir Nextcloud `

`$ cd Nextcloud`

Here you put your docker-compose file of nextcloud, be sure to modify it to your passwords and NFS share first! 

`$ sudo nano docker-compose.yml`

in this file you paste your Nextcloud configuration

ctrl + s and it is saved!

Now at last it is time to run your docker container. You can do this by the following command:

`$ sudo docker-compose up -d`

🎉 **Congrats!!!  Now you should be able to look on Nextcloud and see your external storage as below. **🎉


![image](https://user-images.githubusercontent.com/71763165/156063015-03ebef3b-1159-4b4b-9873-3c07b8c4857b.png)



