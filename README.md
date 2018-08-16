# sql-minikube
Setting up a SQL Server install in Minikube
## Before You Begin
Make sure to enabled virtualization on your machine in your BIOS

## Getting Started
### Install a Hypervisor
If you are using Windows, you will probably be using Hyper-V, which is supported by Minikube, but not all features are fully available.  In this case I'm going to be using VirtualBox, because it supports Persistent Storage.

Download and install VituralBox.  Remember if you have Hyper-V installed you will not be able to use another Hypervisor.

### Install Chocolatey
If you don't have Chocolatey installed on Windows, you probably should.  It is a Package Manager for Windows.
* Open a Admin Powershell
* Now run the following Powershell command to install Chocolatey
> Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
* If you don't see any errors, you are ready to use Chocolatey! Type choco or choco -? now.

### Set the Default Chocolatey Deployment Location

If you don't change the default location that Chocolatey deploys applications without installers, it will create folders under C:\ProgramData\chocolatey\lib

When you use Chocolatey to install kubectl and minikube they should be put in the path.  

### Install kubectl
With Chocolatey installing the Kubernetes CLI is as simple as:
> choco install kubernetes-cli

Now test to make sure it is installed
> kubectl

### Install Minikube
You can install Minikube the hard way, or the easy way. Here's the traditional, hard way - manually.

Go to https://github.com/kubernetes/minikube/releases and download the Windows Installer for Minikube - minikube-installer.exe

Or you can use Chocolatey
> choco install minikube

Now test to make sure minikube is installed
> minikube

# Ready to Play with Minikube
## Start Minikube
Since the default VM driver is VirtualBox, all you need to do now is to start Minikube.  If you are using Hyper-V it is a bit more complex to setup (see https://medium.com/@JockDaRock/minikube-on-windows-10-with-hyper-v-6ef0f4dc158c) but once you do that, all you need to do is add --vm-driver hyperv to the following command as a parameter

From your Powershell, run
> minikube start

It will take a bit, but eventually Kubernetes will start.  Now we have a mini Kubernetes cluster to play with.

## Kick the tires with Hello Minikube
Before we go off and play with SQL Server, we have to do the obligatory "Hello World" for Kubernetes.

Let's install the Hello World container
> kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080

And expose it to the outside World
> kubectl expose deployment hello-minikube --type=NodePort

Let's check to see if it is Ready
> kubectl get pod

It should look something like this

    NAME                              READY     STATUS    RESTARTS   AGE
    hello-minikube-7c77b68cff-r4bbh   1/1       Running   0          1m

If it READY is 0/1 it isn't started yet.

Now that it is running, let's just do a simple curl to it:
> curl $(minikube service hello-minikube --url)

OK, so we are done playing with Hello World, so let's destroy it.  Both the Container and the Service
> kubectl delete services hello-minikube

> kubectl delete deployment hello-minikube

## Install SQL Server

## Shutdown Minikube
Once you have all the services and containers deleted, you can stop the Minikube cluster
> minikube stop
