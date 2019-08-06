# Nice Jenkins docker image details
This image was created from an Ubuntu base image (from dockerhub). 
Jenkins was installed on top of it, configured with git, maven and some plugins. A pipeline job was created .
After the changes were commited, the new image was built and pushed to a dockerhub public repository.
### Running Jenkins container using this image
There are 2 options:
##### Option A

Simply run:
``` sh
docker run -d -p 1234:1234 arikperl/nice-hw:jenkins-arik
```

This will download the image from dockerhub and run it with port 1234 exposed to the host.
If you are running Docker on a your own machine you can access Jenkins' admin console at:
http://localhost:1234
If you wish to change the port exposed by the container, just change the first number on the "-p" part, for example:
``` sh
docker run -d -p 90:1234 arikperl/nice-hw:jenkins-arik
```

Will expose port 90 to the host machine.
If you are running the container on a remote machine, change "localhost" to the IP address/DNS name of the remote host and make sure the exposed port is accessible (FW/SG/etc.)

##### Option B
Make sure docker-compose is installed. It is instlled by default on Windows Docker Desktop. 
On Linux (ubuntu for example) run:
```sh
$ sudo apt install docker-compose
```
Create/copy a file named: "docker-compose.yml" in a dedicated directory which will contain the following:
```sh
version: '3'
services:
  jenkins:
    image: "arikperl/nice-hw:jenkins-arik"
    ports:
      - "1234:1234"
```

Then, you can run the container by typing (while in the same folder as "docker-compose.yml" file):
``` sh
docker-compose up
```

The same instructions regrding exposed port and IP address where you can access Jenkins apply here (port is changed in the docker-compose YAML file).

## Managing and running piplines
There is one pipline defined, called: maven-samples.
This pipline runs the script:
``` sh
node {
  git url: 'https://github.com/gabrielf/maven-samples.git'
  def mvnHome = tool 'M3'
  sh "${mvnHome}/bin/mvn -B verify"
}
```
