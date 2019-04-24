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
