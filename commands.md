# Setup GCE and Enable Cloud Shell
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
