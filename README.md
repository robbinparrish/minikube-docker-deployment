## Disclaimer.
The content on this account/repository provided solely for educational and informational purposes.
It is not intended for use in making any kind of business, investment and/or legal decisions.
Although every effort has been made to keep the information up-to-date and accurate, no representations and/or warranties, express and/or implied, completeness, accuracy, reliability, suitability, and/or availability of the content.

## Minikube Docker Deployment.
Setup local k8s cluster in Docker using [minikube](https://github.com/kubernetes/minikube) for development or test purpose.

## Prerequisites.
- Linux System
- Docker
- wget, curl utilities

NOTE - Some steps requires super user permissions and access to docker daemon resources. If runs as non-root user then use sudo.

## Installation.
- Minikube -
    - wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 -O /usr/bin/minikube
    - chmod +x /usr/bin/minikube
    - minikube version
- kubectl -
    - wget https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl -O /usr/bin/kubectl
    - chmod +x /usr/bin/kubectl
    - kubectl version
- helm -
    - wget https://get.helm.sh/helm-v3.14.2-linux-amd64.tar.gz -O /tmp/helm.tar.gz
    - tar -xvf /tmp/helm.tar.gz -C /tmp/
    - mv /tmp/linux-amd64/helm /usr/bin/helm
    - chmod +x /usr/bin/helm
    - helm version

## Create local k8s cluster.
The below command will download default required images from public registries, this may take some time depending on the network speed.
If require then update the CPU and Memory.

- minikube start --driver=docker --listen-address=0.0.0.0 --cpus 4 --memory 4096

#### Interacting with k8s cluster.
- minikube status
- minikube image ls --format table
- kubectl cluster-info
- kubectl get nodes -o wide
- kubectl get pods -A
- kubectl get svc -A
- kubectl get deployment -A

#### Accessing the k8s dashboard.
Copy the URL from the below command output and use it to access the dashboard in the browser.
Currently this is only available to access in the same VM.
- minikube dashboard --url --port=43434

By default all services deployed in minikube will only be available to local system.

#### Deleting local k8s cluster.
This will purge the all setup configured by Minikube.
- minikube delete --purge

## Deploying nginx application using kubectl.
- kubectl create deployment nginx-app --image=nginx
- kubectl get deployment
- kubectl expose deployment nginx-app --type=NodePort --port=80
- kubectl get svc nginx-app
- kubectl get pods -A
- minikube service nginx-app --url

## Deploying nginx application using helm chart.
Helm tool default uses [Artifact Hub](https://artifacthub.io/) repository.
- helm repo add bitnami https://charts.bitnami.com/bitnami
- helm install nginx-helm-app bitnami/nginx
- kubectl get deployment
- kubectl get svc nginx-helm-app
- kubectl get pods -A
- minikube service nginx-helm-app --url

## Documents links.
- [Minikube](https://minikube.sigs.k8s.io/docs/)
- [kubectl](https://kubernetes.io/docs/reference/kubectl/quick-reference/)
- [helm](https://helm.sh/docs/)
