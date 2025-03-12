**This is a repository for building a Three Node Cluster in K8s.**

I am not a seasoned DevOps person but an enthusiast and love to learn new things. 

Always in the mode of learning.

First things first, decide where you are planning on to install **Docker** and their relative clusters-nodes-pods-containers. Containers are running all the time which will consume your resources such as **CPU**, **MEM** which does so depending on the application that are active in foreground which enforces the container to run at the back. However, you can manage it accordingly by installing them to another drive with enough space to accumulate. Personally, I have used another drive other than 'C:\\' (By default most of the installation points to the poor fella).

In here I shall explain the steps taken into development of a **Three Node Cluster in Kubernetes**.

**Step1: Install Kind**
  - This is one tool that is the backbone in creation of kubernetes clusters.
  - Kind is known to be k8s in docker.
  - Prior to installation of **kind** if any containers to be removed, to free up space then use (I have used this because my system had very little disk space and resources were being       used insanely):

    [bash]``````$ docker rm -f $(docker ps -aq)``````
    --Shows the status of all containers that are running/stopped and force remove them.

    **Or**

    [bash]``````$ docker container prune -f``````
    --For removing idle containers.
  
  - Install **kind**:
    [bash]``````$ curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.27.0/kind-windows-amd64``````
    --Uses curl to install kind executable bin from the URL passed to save kind to your local system
    --'-L' instructs the curl to follow any redirects and 'o' is the output format in this case- **kind-windows-amd64.exe**

    [bash]``````$ choco install kind -y``````
    --To install kind binaries using a package manager. This the standard package manager for windows.
    --If you are debian/ubuntu you could rely on apt.

**Step2: Install kubectl**
  - From the name its 'kubernetes CLI' used to pass commands to interact with the API to manage the clusters.
  - Manage the resources that are shared within the clusters.
  - A cluster has several resources that are being shared among applications. It would be services, deployment, logs, network, storage etc.
    [bash]``````$ curl.exe -Lo "https://dl.k8s.io/v1.32.0/bin/windows/amd64/kubectl.exe.sha256"`````` --To install the executable
    
