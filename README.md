This is repository for building a Three Node Cluster in K8s.

I am not a seasoned DevOps person but a cloud enthusiast and love to learn new things. 

Always in the mode of learning. 

In here I shall explain the steps taken into delivering this personal playground.

**Step1: Install Kind**
  - This is one tool that is the backbone in creation of kubernetes clusters.
  - Kind is known to be k8s in docker.
  - Prior to installation of **kind** if any containers to be removed, to free up space then use (I have used this because my system had very little disk space and resources were being used insanely:

    $ docker rm -f $(docker ps -aq)
    --#Shows the status of all containers that are running and stopped

  - 
