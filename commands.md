# Get the Source Code

## Setup GCE and Enable Cloud Shell
In this section you will create a Google Compute Engine (GCE) account. GCE will allow you to the create VMs, Networks, and Storage volumes required for this workshop. GCE also provides the [Cloud Shell](https://cloud.google.com/shell/docs) computing environment that will be used complete the labs.

## Create a GCE Account
A Google Cloud Platform account is required for this course. You can use an existing GCP account or [sign up for a new one](https://console.cloud.google.com/freetrial) with a valid Gmail account.

A credit card is required for Google Cloud Platform, but there is no subscription cost, you pay only for resources you use.

## Create a Project
A GCP project is required for this course. You can use an existing GCP project or [create a new one](https://support.google.com/cloud/answer/6251787).

Your project name maybe different from your project id.

## Enable Compute Engine and Container Engine APIs
In order to create the cloud resources required by this workshop, you will need to enable the following APIs using the [Google API Console](https://developers.googleblog.com/2016/03/introducing-google-api-console.html):
* Compute Engine API
* Container Engine API

## Enable and explore Cloud Shell
[Google Cloud Shell](https://cloud.google.com/shell/docs) provides you with command-line access to computing resources hosted on Google Cloud Platform and is available now in the Google Cloud Platform Console. Cloud Shell makes it easy for you to manage your Cloud Platform Console projects and resources without having to install the Google Cloud SDK and other tools on your system. With Cloud Shell, the Cloud SDK gcloud command and other utilities you need are always available when you need them.

## Explore Google Cloud Shell
Visit the [Google Cloud Shell getting started guide](https://cloud.google.com/shell/docs/quickstart) and work through the exercises.

## Configure Your Cloud Shell Environment
Create two Cloud Shell Sessions and run the following commands to avoid setting the compute zone.

List available time zones:
```bash
gcloud compute zones list
```

Set a time zone example:
```bash
gcloud config set compute/zone europe-west1-d
```

## Download Go:
**Note:** Cloud Shell comes with an installed Go, but it's not the most recent version, so you should perform the steps below to install the latest Go and set GOPATH.
```bash
wget https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.6.2.linux-amd64.tar.gz
echo "export GOPATH=~/go" >> ~/.bashrc
source ~/.bashrc
```

Get the code:
```bash
mkdir -p $GOPATH/src/github.com/udacity
cd $GOPATH/src/github.com/udacity
git clone https://github.com/udacity/ud615
```

Get ready for the next lesson
```bash
cd ud615/app
```

# Build and Interact with Monolith

## On shell 1 - build the app:
Make sure you are in the app directory and build the app:
```bash
cd $GOPATH/src/github.com/udacity/ud615/app
mkdir bin
go build -o ./bin/monolith ./monolith
```
Optional - if you run into errors building your go binaries, you probably need to install the dependencies first by running:
```bash
$ go get -u
```

## On shell 1 - run the monolith server:
```bash
sudo ./bin/monolith -http :10080
```

## On shell 2 - test the app:
```bash
curl http://127.0.0.1:10080
curl http://127.0.0.1:10080/secure
```

## On shell 2 - authenticate (password is password):
```bash
curl http://127.0.0.1:10080/login -u user
```
It prints out the token.

You can copy and paste the long token in to the next command manually, but copying long, wrapped lines in cloud shell is broken. To work around this, you can either copy the JWT token in pieces, or - more easily - by assigning the token to a shell variable as follows

## On shell 2 - login and assign the value of the JWT to a variable
```bash
TOKEN=$(curl http://127.0.0.1:10080/login -u user | jq -r '.token')
```
Check that it worked:
```bash
echo $TOKEN
```

## On shell 2 - access the secure endpoint using the JWT:
```bash
curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
```

## On shell 2 - check out dependencies
```bash
ls vendor 
cat vendor/vendor.json
```

# Refactor to MSA

## Shell 1 - build and run the hello service
```bash
go build -o ./bin/hello ./hello
sudo ./bin/hello -http 0.0.0.0:10082 -health :10083
```

## Shell 2 - build and run the auth service
```bash
go build -o ./bin/auth ./auth
sudo ./bin/auth -http :10090 -health :10091
```

## Shell 3 - interact with the auth and hello microservices
```bash
TOKEN=$(curl 127.0.0.1:10090/login -u user | jq -r '.token')
curl -H "Authorization:  Bearer $TOKEN" http://127.0.0.1:10082/secure
```

# Installing apps with native OS tools

## Cloud shell - set compute/zone
**Note** - Google Cloud shell is an ephemeral instance and will reset if you don't use it for more than 30 minutes. That is why you might have to set some configuration values again
```bash
gcloud compute zones list
gcloud config set compute/zone <zone>
```

## Cloud shell - launch a new VM instance
```bash
gcloud compute instances create ubuntu \
--image-project ubuntu-os-cloud \
--image ubuntu-1604-xenial-v20160420c
```

## Cloud shell - log into the VM instance
```bash
gcloud compute ssh ubuntu
```

## VM instance - update packages and install nginx
```bash
sudo apt-get update
sudo apt-get install nginx
nginx -v
```

## VM instance - start nginx
```bash
sudo systemctl start nginx
```
Check that it's running
```bash
sudo systemctl status nginx
curl http://127.0.0.1
```

# Problem! How to install two versions

## VM instance
Install nginx again
```bash
sudo apt-get install nginx
```

Start nginx again and check nginx status
```bash
sudo systemctl start nginx
sudo systemctl status nginx
sudo ps aux | grep nginx
```

Check nginx config file
```bash
cat /etc/init/nginx.conf
```

Stop nginx
```bash
sudo systemctl stop nginx
```

# Installing Images with Docker

## Install Docker
```bash
sudo apt-get install docker.io
```

## Check Docker images
```bash
sudo docker images
```

## Pull nginx image
```bash
sudo docker pull nginx:1.10.0
sudo docker images
```

## Verify the versions match
```bash
sudo dpkg -l | grep nginx
```

If your version of nginx from native package and Docker are different, you need to update the VM instance:
```bash
sudo apt-get update
sudo apt-get install nginx
```

# Running Images with Docker

## Run the first instance
```bash
sudo docker run -d nginx:1.10.0
```

## Check if it's up
```bash
sudo docker ps
```

## Run a different version of nginx
```bash
sudo docker run -d nginx:1.9.3
```

## Run another version of nginx
```bash
sudo docker run -d nginx:1.10.0
```

## Check how many instances are running
```bash
sudo docker ps
sudo ps aux | grep nginx
```

## What's with the container names?
If you don't specify a name, Docker gives a container a random name, such as "stoic_williams," "sharp_bartik," "awesome_murdock," or "evil_hawking." (Stephen Hawking got no love on this one.)

These are generated from a list of adjectives and names of famous scientists and hackers. The combination of the names and adjectives is random, except for one case. Want to see what the exception is? Check it out in the [Docker source code](https://github.com/docker/docker/blob/master/pkg/namesgenerator/names-generator.go)!


# Talking to Docker instances

## List all running container processes
```bash
sudo docker ps
```
For use in shell scripts you might want to just get a list of container IDs (-a stands for all instances, not just running, and -q is for "quiet" - show just the numeric ID):
```bash
sudo docker ps -aq
```

## Inspect the container
You can use either CONTAINER ID or NAMES field, for example for a sudo docker ps output like this:

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
f86cf066c304        nginx:1.10.0        "nginx -g 'daemon off"   8 minutes ago       Up 8 minutes        80/tcp, 443/tcp     sharp_bartik
You can use either of the following commands:
```bash
sudo docker inspect f86cf066c304
```
or
```bash
sudo docker inspect sharp_bartik
```

## Connect to the nginx using the internal IP
Get the internal IP address either copying from the full inspect file or by assigning it to a shell variable:
```bash
CN="sharp_bartik"
CIP=$(sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' $CN)
curl  http://$CIP
```
You can also get all instance IDs and their corresponding IP addresses by doing this:
```bash
sudo docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(sudo docker ps -aq)
```

## Stop an instance
```bash
sudo docker stop <cid>
or sudo docker stop $(sudo docker ps -aq)
```

## Verify no more instances running
```bash
sudo docker ps
```

## Remove the docker containers from the system
```bash
sudo docker rm <cid>
```
or 
```bash
sudo docker rm $(sudo docker ps -aq)
```

# Create An Image

## Install Go
```bash
wget https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz
rm -rf /usr/local/bin/go
sudo tar -C /usr/local -xzf go1.6.2.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
export GOPATH=~/go
```

## Get the app code
```bash
mkdir -p $GOPATH/src/github.com/udacity
cd $GOPATH/src/github.com/udacity
git clone https://github.com/udacity/ud615.git
```

## Build a static binary of the monolith app
```bash
cd ud615/app/monolith
go get -u
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
```

## Why did you have to build the binary with such an ugly command line?
You have to explicitly make the binary static. This is really important in the Docker community right now because alpine has a different implementation of libc. So your go binary wouldn't have had the lib it needed if it wasn't static. You created a static binary so that your application could be self-contained.

## Create a container for the app
Look at the Dockerfile
```bash
cat Dockerfile
```
Build the app container
```bash
sudo docker build -t monolith:1.0.0 .
```
List the monolith image
```bash
sudo docker images monolith:1.0.0
```

## Run the monolith container and get it's IP
```bash
sudo docker run -d monolith:1.0.0
sudo docker inspect <container name or cid>
```
or
```bash
CID=$(sudo docker run -d monolith:1.0.0)
CIP=$(sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' ${CID})
```

## Test the container
```bash
curl <the container IP>
```
or
```bash
curl $CIP
```

## Important note on security
If you are tired of typing "sudo" in front of all Docker commands, and confused why a lot of examples don't have that, please read the following article about implications on security - [Why we don't let non-root users run Docker in CentOS, Fedora, or RHEL](http://www.projectatomic.io/blog/2015/08/why-we-dont-let-non-root-users-run-docker-in-centos-fedora-or-rhel/)

# Create the Other Containers

## Create docker images for the remaining microservices - auth and hello.
Repeat the steps you took for monolith.

## Build the auth app
```bash
cd $GOPATH/src/github.com/udacity/ud615/app
cd auth
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
sudo docker build -t auth:1.0.0 .
CID2=$(sudo docker run -d auth:1.0.0)
```

## Build the hello app
```bash
cd $GOPATH/src/github.com/udacity/ud615/app
cd hello
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
sudo docker build -t hello:1.0.0 .
CID3=$(sudo docker run -d hello:1.0.0)
```

## See the running containers
```bash
sudo docker ps -a
```

# Push Images

## See all images
```bash
sudo docker images
```
Docker tag command help
```bash
docker tag -h
```

## Add your own tag
```bash
sudo docker tag monolith:1.0.0 <your username>/monolith:1.0.0
```

## For example (you can rename too!)
```bash
sudo docker tag monolith:1.0.0 udacity/example-monolith:1.0.0
```

## Create account on Dockerhub
To be able to push images to Dockerhub you need to create an account there - https://hub.docker.com/register/

## Login and use the docker push command
```bash
sudo docker login
sudo docker push udacity/example-monolith:1.0.0
```

Repeat for all images you created - monolith, auth and hello!

# Setting up Kubernetes for this course

## Use project directory
```bash
cd $GOPATH/src/github.com/udacity/ud615/kubernetes
```
or if you are in the course repository:
```bash
cd kubernetes
```
**Note**: At any time you can clean up by running the cleanup.sh script

## Provision a Kubernetes Cluster with GKE using gcloud
To complete the work in this course you going to need some tools. Kubernetes can be configured with many options and add-ons, but can be time consuming to bootstrap from the ground up. In this section you will bootstrap Kubernetes using Google Container Engine (GKE).

GKE is a hosted Kubernetes by Google. GKE clusters can be customized and supports different machine types, number of nodes, and network settings.

Use the following command to create your cluster for use for the rest of this course.
```bash
gcloud container clusters create k0 
```

# Kubernetes Intro Demo

## Launch a single instance:
```bash
kubectl run nginx --image=nginx:1.10.0
```

## Get pods
```bash
kubectl get pods
```

## Expose nginx
```bash
kubectl expose deployment nginx --port 80 --type LoadBalancer
```

## List services
```bash
kubectl get services
```

## Kubernetes cheat sheet
We just went over a lot and we know you’re probably a little overwhelmed. Fear not! We’ll be going over each of these concepts, over the next two lessons. And you can always come back to this demo -- if you need to watch it again.
To help out, here’s a Kubernetes command cheat sheet. http://kubernetes.io/docs/user-guide/kubectl-cheatsheet/

# Creating Pods

## Explore config file
```bash
cat pods/monolith.yaml
```

## Create the monolith pod
```bash
kubectl create -f pods/monolith.yaml
```

## Examine pods
```bash
kubectl get pods
```
It may take a few seconds before the monolith pod is up and running as the monolith container image needs to be pulled from the Docker Hub before we can run it.
Use the kubectl describe command to get more information about the monolith pod.
```bash
kubectl describe pods monolith
```

# Interacting with Pods

## Cloud shell 1: set up port-forwarding
```bash
kubectl port-forward monolith 10080:80
```

## Open new Cloud Shell session 2
```bash
curl http://127.0.0.1:10080
curl http://127.0.0.1:10080/secure
```

## Cloud shell 2 - log in
```bash
curl -u user http://127.0.0.1:10080/login
curl -H "Authorization: Bearer <token>" http://127.0.0.1:10080/secure
```

## View logs
```bash
kubectl logs monolith
kubectl logs -f monolith
```

## In Cloud Shell 3
```bash
curl http://127.0.0.1:10080
```

## In Cloud Shell 2
Exit log watching (Ctrl-C)
You can use the kubectl exec command to run an interactive shell inside the monolith Pod. This can come in handy when you want to troubleshoot from within a container:
```bash
kubectl exec monolith --stdin --tty -c monolith /bin/sh
```
For example, once we have a shell into the monolith container we can test external connectivity using the ping command.
```bash
ping -c 3 google.com
```
When you’re done with the interactive shell be sure to logout.
```bash
exit
```

# Creating Secrets
```bash
ls tls
```
The cert.pem and key.pem files will be used to secure traffic on the monolith server and the ca.pem will be used by HTTP clients as the CA to trust. Since the certs being used by the monolith server where signed by the CA represented by ca.pem, HTTP clients that trust ca.pem will be able to validate the SSL connection to the monolith server.

## Use kubectl
to create the tls-certs secret from the TLS certificates stored under the tls directory:
```bash
kubectl create secret generic tls-certs --from-file=tls/
```
kubectl will create a key for each file in the tls directory under the tls-certs secret bucket. Use the kubectl describe command to verify that:
```bash
kubectl describe secrets tls-certs
```
Next we need to create a configmap entry for the proxy.conf nginx configuration file using the kubectl create configmap command:
```bash
kubectl create configmap nginx-proxy-conf --from-file=nginx/proxy.conf
```
Use the kubectl describe configmap command to get more details about the nginx-proxy-conf configmap entry:
```bash
kubectl describe configmap nginx-proxy-conf
```

## TLS and SSL
TLS and SSL can be confusing topics. Here’s a good primer for understanding the basics: https://en.wikipedia.org/wiki/Transport_Layer_Security
