**This is a repository for building a Three Node Cluster in K8s.**

I am not a seasoned DevOps person but an enthusiast and love to learn new things. 

Always in the mode of learning.

**Sys Spec**
  OS: Win 10 Pro
  Proc: Intel(R) Core(TM) i7-4600U CPU @ 2.10GHz, 2701 Mhz, 2 Core(s), 4 Logical Processor(s)

First things first, decide where you are planning on to install **Docker** and their relative clusters-nodes-pods-containers. Containers are running all the time which will consume your resources such as **CPU**, **MEM** which does so depending on the application that are active in foreground which enforces the container to run at the back. However, you can manage it accordingly by installing them to another drive with enough space to accumulate. Personally, I have used another drive other than 'C:\\' (By default most of the installation points to the poor fella).

In here I shall explain the steps taken into development of a **Three Node Cluster in Kubernetes**.

**Step1: Install kind**
  - This is one tool that is the backbone in creation of kubernetes clusters.
  - Kind is known to be k8s in docker.
  - Prior to installation of **kind** if any containers to be removed, to free up space then use (I have used this because of scant resources):

    [bash] ``````$ docker rm -f $(docker ps -aq)``````
    --Shows the status of all containers that are running/stopped and force remove them.

    **Or**

    [bash] ``````$ docker container prune -f``````
    --For removing idle containers.
  
  - Install **kind**:
    [bash] ``````$ curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.27.0/kind-windows-amd64``````
    --Uses curl to install kind executable bin from the URL passed to save kind to your local system.
    --'-L' instructs the curl to follow any redirects and 'o' is the output format in this case- **kind-windows-amd64.exe**

    [bash] ``````$ choco install kind -y``````
    --To install kind binaries using a package manager. This the standard package manager for windows.
    --If you are debian/ubuntu you could rely on apt.

**Step2: Install kubectl**
  - From the name its 'kubernetes CLI' used to pass commands to interact with the API to manage the clusters.
  - Manage the resources that are shared within the clusters.
  - A cluster has several resources that are being shared among applications. It would be services, deployment, logs, network, storage etc.
    [bash] ``````$ curl.exe "https://dl.k8s.io/release/v1.32.0/bin/windows/amd64/kubectl.exe"`````` --To install the executable
  - Next we need to install check-sum for the corresponding kubectl installed. This is done to verify whether the installed version is not tampered.
    [bash] ``````$ curl.exe -Lo "https://dl.k8s.io/v1.32.0/bin/windows/amd64/kubectl.exe.sha256"``````
    [sh] ``````CertUtil -hashfile kubectl.exe SHA256``````--CertUtil is a windows certificate management tool that calls in for hashing of installed kubectl.exe using the SHA256 algo.
    [sh] ``````type kubectl.exe.sha256`````` --This is the downloaded version of hash for comparison with the one that is generated.
    [sh] ``````[System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\path\to\kubectl\folder", [System.EnvironmentVariableTarget]::Machine)`````` --Enables in setting a        path of the executable file to the 'env'.
    [sh] ``````kubectl version --client`````` --To confirm the installation.
    [sh] ``````kubectl cluster-info`````` --This throws a url of the api used to communicate with it for users and other systems to interact with the cluster.

**Step3: Create cluster**
  - Create a config file in .yaml to define your k8s cluster in it.
    ``````kind: Cluster
          apiVersion: kind.x-k8s.io/v1alpha4
          nodes:
          - role: control-plane
          - role: worker
          - role: worker
    ``````
    --This notifies the api that it is a kind cluster with the version and the nodes going to be ready for work.
    --control-plane is the one that manages the cluster and their components: api server, scheduler, controller manager.
    --worker nodes are the ones that do the work: pods, deployments etc.
    [bash] ``````kind create cluster --config kind-config.yaml`````` --Creates the cluster.
    [bash] ``````kind get clusters`````` --Verify
    [bash] ``````kind get nodes`````` --Displays details of nodes: status, age and ver.
    [bash] ``````kubectl create deployment nginx --image=nginx`````` --Creates a K8s deployment in nginx.
    --In K8s nginx is used for the ingress controller. Ingress is used to re-route the internal traffic of the cluster which hits the services from external IP.
