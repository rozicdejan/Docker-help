# Docker-help
## Configure Docker to start on boot with systemd
Many modern Linux distributions use systemd to manage which services start when the system boots. On Debian and Ubuntu, the Docker service starts on boot by default. To automatically start Docker and containerd on boot for other Linux distributions using systemd, run the following commands:

     sudo systemctl enable docker.service
     sudo systemctl enable containerd.service
To stop this behavior, use disable instead.

     sudo systemctl disable docker.service
     sudo systemctl disable containerd.service
## Create the docker group and add your user:

      sudo groupadd docker
      sudo usermod -aG docker $USER
Log out and log back in so that your group membership is re-evaluated.

If you're running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
## Verify that you can run docker commands without sudo.

      docker run hello-world
      
You can also run the following command to activate the changes to groups:
# Check for docker info 
     
      docker info
# Not sure if your container is up and running
If, after starting your container in step 2, you’re not sure if your container is up and running, you can run the code below:

      docker inspect homeassistantCopy
This code will return all necessary information about your container.
A Docker container consists of network settings, volumes, and images. The location of Docker files depends on your operating system. Here is an overview for the most used operating systems:
# Docker DIR
    Ubuntu: /var/lib/docker/
    Fedora: /var/lib/docker/
    Debian: /var/lib/docker/
    Windows: C:\ProgramData\DockerDesktop
    MacOS: ~/Library/Containers/com.docker.docker/Data/vms/0/
# The internal structure of the Docker root folder
      $ ls -la /var/lib/docker
      
            total 152
            drwx--x--x   15 root     root          4096 Feb  1 13:09 .
            drwxr-xr-x   13 root     root          4096 Aug  1  2019 ..
            drwx------    2 root     root          4096 May 20  2019 builder
            drwx------    4 root     root          4096 May 20  2019 buildkit
            drwx------    3 root     root          4096 May 20  2019 containerd
            drwx------    2 root     root         12288 Feb  3 19:35 containers
            drwx------    3 root     root          4096 May 20  2019 image
            drwxr-x---    3 root     root          4096 May 20  2019 network
            drwx------    6 root     root         77824 Feb  3 19:37 overlay2
            drwx------    4 root     root          4096 May 20  2019 plugins
            drwx------    2 root     root          4096 Feb  1 13:09 runtimes
            drwx------    2 root     root          4096 May 20  2019 swarm
            drwx------    2 root     root          4096 Feb  3 19:37 tmp
            drwx------    2 root     root          4096 May 20  2019 trust
            drwx------   15 root     root         12288 Feb  3 19:35 volumes
# Docker images
The heaviest contents are usually images. If you use the default storage driver overlay2, then your Docker images are stored in /var/lib/docker/overlay2. There, you can find different files that represent read-only layers of a Docker image and a layer on top of it that contains your changes.
# Clean up space used by Docker
       docker system prune -a
# remove unused volumes by executing:
      docker volumes prune
# Docker backup
Docker has become an essential tool for many developers in recent years, allowing them to create and manage containerized applications with ease. One of the most significant advantages of Docker is its ability to manage data volumes, allowing you to store data that persists beyond the lifetime of a container.

However, even with its many benefits, Docker’s volume management can sometimes be challenging. In particular, taking backups and restoring volumes can be a complex and error-prone process. In this article, we’ll explore why you might need to take backups of your volumes and how to do it correctly.
## Disaster Recovery
The most obvious reason to take backups of your Docker volumes is to protect against data loss in the event of a disaster. By taking regular backups of your volumes, you can ensure that your critical data is safe and recoverable in case of an emergency.
## Step 1: Identify the Volume
The first step in taking a backup of a volume is to identify the volume that you want to back up. You can do this by running the following command:

      docker volume ls
This will show you a list of all the volumes that are currently available on your Docker host. Note down the name of the volume that you want to back up.
## Step 2: Create a Backup
To create a backup of the volume, you can use the docker run command to start a container that mounts the volume you want to back up and a separate container that writes the backup data to a file.
Here’s an example of how to do this:

      docker run --rm \
      --mount source=<volume-name>,target=<target> \
      -v $(pwd):/backup \
      busybox \
      tar -czvf /backup/<backup-filename>.tar.gz <target>
In this command, replace <volume-name> with the name of the volume you want to back up, <target> with the mount point inside the docker container, and <backup-filename> with a name for the backup file.
## Step 3: Move the Backup File to an External Server
After you have created a backup file, it’s a good idea to move it to an external server or storage device to ensure that it’s safe and secure. Storing the backup file on a separate server or storage device can help to protect it in the event of a disaster, such as a server failure or a security breach.

To move the backup file to an external server, you can use SCP.

SCP: Secure Copy (SCP) is a secure file transfer protocol that allows you to transfer files between servers using SSH. To use SCP, you will need to have SSH access to both the source and destination servers. You can use the following command to copy the backup file to the external server:

      scp /path/to/backupfile user@external-server:/path/to/destination
## Step 4: Restore the Volume
If you need to restore the volume from the backup, you can use the docker run command to start a container that mounts the backup file and a separate container that writes the backup data to the volume.

Here’s an example of how to do this:

      docker run --rm \
      --mount source=<volume-name>,target=<target> \
      -v $(pwd):/backup \
      busybox \
      tar -xzvf /backup/<backup-filename>.tar.gz -C /
In this command, replace <volume-name> with the name of the volume you want to back up, <target> with the mount point inside the docker container, and <backup-filename> with a name for the backup file.
# Example
We will be setting up a basic Docker environment for MongoDB, along with an Express application to visualize the data.
## Setup MongoDB Docker
Create a docker-compose.yml file.

You can use the below example to setup MongoDB docker:

      # Use root/example as user/password credentials
      version: '3.1'
      services:
        mongo:
          image: mongo
          container_name: mongo
          restart: always
          environment:
            MONGO_INITDB_ROOT_USERNAME: root
            MONGO_INITDB_ROOT_PASSWORD: example
          volumes:
            - mongodb:/data/db
        mongo-express:
          image: mongo-express
          container_name: mongo-express
          restart: always
          ports:
            - 8081:8081
          environment:
            ME_CONFIG_MONGODB_ADMINUSERNAME: root
            ME_CONFIG_MONGODB_ADMINPASSWORD: example
            ME_CONFIG_MONGODB_URL: mongodb://root:example@mongo:27017/
      volumes:
        mongodb: ~
In the above example, I added a named volume mongodb. Here we have configured docker to persist its data to a volume.

      docker-compose up
To check the running docker containers you can use the below command.

      docker ps
You should see 2 docker containers running i.e. mongo and mongo-express.

      CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS          PORTS                                       NAMES
      5f68eaf8184a   mongo-express   "tini -- /docker-ent…"   11 seconds ago   Up 9 seconds    0.0.0.0:8081->8081/tcp, :::8081->8081/tcp   mongo-express
      6789cf1e7bed   mongo           "docker-entrypoint.s…"   11 seconds ago   Up 10 seconds   27017/tcp                                   mongo
You can also view the volume which you created using the following command.

      docker volume ls
For me the output looks like this

      DRIVER    VOLUME NAME
      local     82fd19dfc5dc839a51b2afd59614474038cd0ce8b494739d883d9b503cd0c61e
      local     32582d9a00557c9229418062e6308515c4c4c7eb0bb14a8c43203cafffb675c5
      local     docker-test_mongodb
The volume name docker-test_mongodb was automatically created when I placed my docker-composer.yml file inside the docker-test folder. This is the location where all of our MongoDB data will be stored.
Modify MongoDB Data
To confirm that we have successfully restored the database from the volume, we need to make a minor modification to the database.

      Visit 0.0.0.0:8081 where the Mongo Express is running.
      Create a new database here. I created my-new-db as the new database.
## Take Backup
Let’s take the backup of the volume now.

Please run the following command to take the backup.

      docker run --rm \
        --mount source=docker-test_mongodb,target=/data/db \
        -v $(pwd):/backup \
        busybox \
        tar -czvf /backup/docker-test_mongodb.tar.gz /data/db
      /data/db here is the path that is mounted inside the docker container for the volume.

You should see a backup file created with docker-test_mongodb.tar.gz as the file name.
## Reset Docker
As I will be performing the restore process on the same machine, I should delete both the container and the volume.

      docker rm -v mongo-express
      docker rm -v mongo
      docker volume rm docker-test_mongodb
## Restore Backup
I will proceed to recreate the containers, and we can confirm that the volume has been deleted by checking the databases on Mongo Express.
Now we can run the below command to restore the backup.

      docker run --rm \
      --mount source=docker-test_mongodb,target=/data/db \
      -v $(pwd):/backup \
      busybox \
      tar -xzvf /backup/docker-test_mongodb.tar.gz -C /
## Conclusion
Taking backups and restoring volumes in Docker can be a complex and error-prone process. However, with the right approach, it’s possible to ensure that your critical data is safe and recoverable in the event of a disaster. By following the steps outlined in this article, you can take backups of your volumes with ease and restore them when necessary.
