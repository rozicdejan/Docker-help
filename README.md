# Docker-help
# Check for docker info 
      docker info
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
           
