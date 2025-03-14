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
    - Shows the status of all containers that are running/stopped and force remove them.

    **Or**

    [bash] ``````$ docker container prune -f``````
    - For removing idle containers.
  
  - Install **kind**:
    [bash] ``````$ curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.27.0/kind-windows-amd64``````
    - Uses curl to install kind executable bin from the URL passed to save kind to your local system.
    - '-L' instructs the curl to follow any redirects and 'o' is the output format in this case- **kind-windows-amd64.exe**

    [bash] ``````$ choco install kind -y``````
    - To install kind binaries using a package manager. This the standard package manager for windows.

    - If you are debian/ubuntu you could rely on apt.

**Step2: Install kubectl**
  - From the name its 'kubernetes CLI' used to pass commands to interact with the API to manage the clusters.
  - Manage the resources that are shared within the clusters.
  - A cluster has several resources that are being shared among applications. It would be services, deployment, logs, network, storage etc.

    [bash] ``````$ curl.exe "https://dl.k8s.io/release/v1.32.0/bin/windows/amd64/kubectl.exe"``````
    - To install the executable
  
  - Next we need to install check-sum for the corresponding kubectl installed. This is done to verify whether the installed version is not tampered.

    [bash] ``````$ curl.exe -Lo "https://dl.k8s.io/v1.32.0/bin/windows/amd64/kubectl.exe.sha256"``````

    [sh] ``````CertUtil -hashfile kubectl.exe SHA256``````
    - CertUtil is a windows certificate management tool that calls in for hashing of installed kubectl.exe using the SHA256 algo.

    [sh] ``````type kubectl.exe.sha256``````
    - This is the downloaded version of hash for comparison with the one that is generated.

    [sh] ``````[System.Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\path\to\kubectl\folder", [System.EnvironmentVariableTarget]::Machine)``````
    - Enables in setting a path of the executable file to the 'env'.

    [sh] ``````kubectl version --client``````
    - To confirm the installation.

    [sh] ``````kubectl cluster-info``````
    - This throws a url of the api used to communicate with it for users and other systems to interact with the cluster.

**Step3: Create cluster**
  - Create a config file in .yaml to define your k8s cluster in it.
    ``````kind: Cluster
          apiVersion: kind.x-k8s.io/v1alpha4
          nodes:
          - role: control-plane
          - role: worker
          - role: worker
    ``````
    - This notifies the api that it is a kind cluster with the version and the nodes going to be ready for work.
    - control-plane is the one that manages the cluster and their components: api server, scheduler, controller manager.
    - worker nodes are the ones that do the work: pods, deployments etc.
    
    [bash] ``````kind create cluster --config kind-config.yaml``````
    - Creates the cluster.

    [bash] ``````kind get clusters``````
    - Verify

    [bash] ``````kind get nodes``````
    - Displays details of nodes: status, age and ver.

    [bash] ``````kubectl create deployment nginx --image=nginx``````
    - Creates a K8s deployment in nginx.
    - In K8s nginx is used for the ingress controller. Ingress is used to re-route the internal traffic of the cluster which hits the services from external IP.
    - In this case it takes the nginx image to deploy.

    [bash] ``````kubectl expose deployment nginx --type=NodePort --port=80``````
    - Exposes the nginx to a port.
    - This is the port through which all single point of requests from external sources would be coming in for the services.
    - Now you can check your deployments, pods that has been created.
      
    [sh] ``````kubectl get deployments``````

    [sh] ``````kubectl describe deployment nginx``````
    - For a detailed decription of the deployments.

    [sh] ``````kubectl get pods``````

**Init, create, add, push a repo**
  - Initialize a repo on the root folder.
    [sh] ``````cd Drive/Main_Folder/Sub_Folder/root git init``````

    [bash] ``````touch .gitignore`````` --Create a .gitignore on the root
    - This file is created to add untracked files into them.
    - They have logs, swaps, env, pycache and node_modules as these get heavy in size and exposing highly sensitive data to version control becomes a security concern.

    - Create a repo at github.
    
    [sh] ``````git remote add origin https://github.com/<user>/<repo-name>.git``````
    - Now add the remote repo to your local system in the name origin.
    
    [sh] ``````git add .``````
    - Add changes to the github repo.
    
    [sh] ``````git commit -m "initial commit: namespace"``````
    - Commit them.
  
    [sh] ``````git branch -M main`````` --This renames the branch to main.
    - Create a branch of local origin repo to remote repo as main.
  
    [sh] ``````git push -u origin main``````
    - Push all upstreams to the new main.
    
    [sh] ``````git push origin main``````

    [sh] ``````git remote -v``````
    - Verify the remote repos (To do: while in root folder).
  
    [sh] ``````git branch -r``````
    - Verify the branch.
    - 
**Install Helm**
  - I have used helm here because of the efficiency in managing the K8s applications for further enhancements, if required.
  - It contains all the resources required to run an application and assists in automating through simple commands as all the configs are combined in a single reusable file.

    [bash] ``````curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash``````
    - Installs it from git and pushes into bash to run the installation.

    [sh] ``````helm version`````` --Verify the version

  - Create helm chart.
    [sh] ``````helm create helm-chart``````

    [sh] ``````helm dependency update helm-chart``````
    - Update dependencies.
      
    - Prior to adding to git you need to make sure you have user access.
    - Troubleshoot by adding one.

    [sh] ``````git config --global user.name "<user-name>"``````

    [sh] ``````git commit -m "helm chart"``````

    [sh] ``````git push origin main``````

    - In my case this threw an error pointing that user pass has been removed and not able to identify.

    - I had to manually generate a token by going to github (Ref:PAT).

    - Push the generated token to github.

    [sh] ``````git remote set url-origin https://eshnish:<token_key>@github.com/user/<repo-name>.git``````

    [sh] ``````git push origin main``````

    - At times when the token has to be changed then you need to pull the token prior to a push.

    [sh] ``````git pull origin main``````

    [sh] ``````git branch -M main``````

    [sh] ``````git push -u origin main``````

    [sh] ``````git push origin main``````

    --**The output throws the directories to be up-to-date**

**Step4: Generate SSH key**
  - SSH are used to access repos w/o any password authentication. private:public (local_system:github) keys.
  - Authentication occurs by verifying the private in local system to the public key in github.
  - Used to convenience for the users.

    [sh] ``````ssh-keygen -t ed25519 -C "email@mail.com"``````
    - Uses the most recommended key such as ed25519 and throws a comment that gets added to the public key stored at github for identification purposes.

    [bash] ``````cat ~/.ssh/id_ed25519.pub``````
    - Displays the public ssh-id.
    - Go to github and paste the key and Save it in SSH key settings.
      **Note: if you do not paste it to git then the system will throw error!**

    [sh] ``````ssh -T git@github.com``````
    - Testing connection.

    [sh] ``````git remote -v``````
    - Checking the key with remote repo.

    [sh] ``````git push origin main``````
    - Pushing the key to github repo.
   
**Step5: Create Kustomize.yaml file**
  - In this case I have added Kustomize as they function similar to Helm. You don't need a helm engine when there is Kustomize.yaml. Added it for understanding purpose only.
  - Kustomize is a template-free config file that has details of all the resources, customization.
  - They also generates config map and secrets from other environments.
  - Therefore, they are similar to helm-charts.
    - Create a folder at the root.
    - Create a kustomization.yaml.
    [sh] ``````New-Item -Path D:\path\file\to\folder\<kustomization.yaml> -ItemType File``````
      
    - Add syntax.
    ``````apiVersion: kustomize.config.k8s.io/v1beta1
          kind: Kustomization
          resources:
          - deployment.yaml                                                                                                   
          - service.yaml
    ``````
