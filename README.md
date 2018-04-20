# PREREQUISITES

This lab requires the following:
* Free login Docker ID: https://cloud.docker.com/
* Free Oracle login account: https://profile.oracle.com/myprofile/account/create-account.jspx
* Laptop or tablet with a web browser capable of running Play with Docker VM sessions: https://labs.play-with-docker.com/ (most modern, up-to-date browsers will do the trick)

# PART 1: SETUP AND IMAGE DOWNLOADS

Docker Labs provides a web-based, lightweight laboratory environment called Play with Docker. This platform is completely free, and will work with most modern, up-to-date browsers. **Please note that VMs are automatically deleted and unrecoverable after 4 hours, so do not store any critical information or lab notes directly on the Play with Docker instances.**

__Alternatively__: The Play with Docker VMs include all of the necessary software pre-installed. The lab will also work on any system with Docker and Docker Compose version 17.12 or later. However, the system might not meet the minimum specifications to run the Docker containers, as has been validated with the Play with Docker VMs.

1. In a web browser, navigate to the Play with Docker site via https://labs.play-with-docker.com/
2. On the home page, navigate to Login > docker:
 
3.	Login using a Docker ID and password.

4.	Click Start:

5.	In the lab window, click Add New Instance:

6.	The console for the new instance will appear. Note that the 4-hour counter has started in the upper left of the screen. Also, if desired, the instance can be connected to via an SSH client such as PuTTY.

7.	The instance is already logged into the Docker Hub, which stores the MySQL container. Download the latest MySQL container instance using the pull option of the docker command:

```
$ docker pull mysql:latest

latest: Pulling from library/mysql
2a72cbf407d6: Pull complete
38680a9b47a8: Pull complete
4c732aa0eb1b: Pull complete
c5317a34eddd: Pull complete
f92be680366c: Pull complete
e8ecd8bec5ab: Pull complete
2a650284a6a8: Pull complete
5b5108d08c6d: Pull complete
beaff1261757: Pull complete
c1a55c6375b5: Pull complete
8181cde51c65: Pull complete
Digest: sha256:691c55aabb3c4e3b89b953dd2f022f7ea845e5443954767d321d5f5fa394e28c
Status: Downloaded newer image for mysql:latest
```

8.	Due to license restrictions regarding the distribution of the WebLogic installation media, the WebLogic 12.2.1.3 container must be downloaded directly from the Oracle Container Registry. Use the login option of the docker command to login to the Oracle Container Registry with valid Oracle credentials:

```
$ docker login container-registry.oracle.com

Username: ORACLE USERNAME
Password: ORACLE PASSWORD
Login Succeeded
```

9.	After logging into the registry, download the 12.2.1.3 version of the WebLogic container using the pull option of the docker command (NOTE: this image is about 1.1 GB and may take several minutes to download):

```
$ docker pull container-registry.oracle.com/middleware/weblogic:12.2.1.3

12.2.1.3: Pulling from middleware/weblogic
8dc3ae811b63: Pull complete
4efd7b91b3ef: Pull complete
9f8c6a66986c: Pull complete
9e565d6ad1d3: Pull complete
c46907c1dea7: Pull complete
e378102878a1: Pull complete
bc0642354402: Pull complete
Digest: sha256:23d9048f6eb73bd16f02a63bf78427bc6c1cb9cb3eba149e6da1173ae1e9bf1d
Status: Downloaded newer image for container-registry.oracle.com/middleware/weblogic:12.2.1.3
```

10.	The Oracle GitHub repository contains scripts for building Docker images from original installation media for many Oracle products, including Database, Web Tier, and GoldenGate. The repository also contains several sample Dockerfiles and Docker Compose artifacts. To simplify the scripting involved, this lab will use a sample Dockerfile from the GitHub repository that builds a Docker image containing a simple WebLogic domain.

Clone the Oracle GitHub repository and change to the WebLogic directory containing the domain sample:

```
$ git clone https://github.com/oracle/docker-images.git

Cloning into 'docker-images'...
remote: Counting objects: 7811, done.
remote: Compressing objects: 100% (90/90), done.
remote: Total 7811 (delta 40), reused 84 (delta 33), pack-reused 7688
Receiving objects: 100% (7811/7811), 9.73 MiB | 20.08 MiB/s, done.
Resolving deltas: 100% (4308/4308), done.

$ cd docker-images/OracleWebLogic/samples/12213-domain
```

11.	In a text editor such as vi, edit the file name Dockerfile. replace the following line:

```FROM oracle/weblogic:12.2.1.3-developer```

With the image downloaded from the Oracle Container Registry:

```FROM container-registry.oracle.com/middleware/weblogic:12.2.1.3```

12.	Use the **build** option of the docker command to create a Docker container image containing a simple domain, named _12213-domain_:

```
$ docker build -t 12213-domain .

*snip*

Successfully tagged 12213-domain:latest
```

13.	The Vertical Trail GitHub repository contains a sample domain properties file, plus a sample application that can be used to validate the full Oracle stack once is has been deployed. Change back to the home directory and clone this repository:

```$ git clone https://github.com/verticaltrail/c18lv-XXX-lab.git```

The environment is now ready to build an Oracle stack containing a WebLogic environment with a MySQL database!
