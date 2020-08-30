# Kubernetes
```
Kubernetes is an opensource platform development by Google and used to create cluster.

The cluster is composed by one or more containers and the containers are wrapped in pods.
In Kubernetes we have the master and nodes.

Pods are the most basic objects in Kubernetes and therefore, there are no resources and no desired state of our application to Kubernetes manager.
In Kubernetes it performs the abstraction of Pods objects in objects that offer more resources, such as the Deployment object that is capable of adding the desired state of the application to the manageable Kubernetes, services, statefulset, etc.

Some concepts:
Pod is the smallest deploy object in Kubernetes;
Pod is the object that defines how to run the container (image, network, volumes, ports);
A Pod alone does not guarantee the desired state;
Deployment: provides declarative updates for Pods;
Services: an abstract way to expose an application running on a set of Pods as a network service;
Statefulset: is the workload API object used to manage stateful applications.

In this example it was used Minikube, a tool that makes it easy to run Kubernetes.

In this project it was deployed Wordpress application (3 replicas) connect with Mysql (data are stored in volume).

```

# Install minikube
```
https://kubernetes.io/docs/tasks/tools/install-minikube/
```

1. In BIOS, disable "Secure Boot"

2. Install Virtualbox
```
https://phoenixnap.com/kb/install-virtualbox-on-ubuntu
```

3. Install curl
```
sudo apt-get install curl
```

4. Download minikube
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && sudo chmod +x minikube && sudo mv minikube /usr/local/bin/
```

5. Download kubectl
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```

6. Execute mode
```
chmod +x ./kubectl
```

7. Move kubectl to environment variables
```
sudo mv ./kubectl /usr/local/bin/kubectl 
```

# Uninstall minikube
```
minikube stop
minikube delete
docker stop $(docker ps -aq)
rm -rf ~/.kube ~/.minikube
sudo rm /usr/local/bin/localkube /usr/local/bin/minikube /usr/local/bin/kubectl
systemctl stop '*kubelet*.mount'
sudo rm -rf /etc/kubernetes/
docker system prune -af --volumes
```

# Configure Kubernetes cluster and kubernetes commands
https://minikube.sigs.k8s.io/docs/start/

1. Starting minikube cluster
```
minikube start

# Increase performance
a. Use logs command minikube logs

b. Because Minikube is working on Virtual Machine sometimes is better to delete minikube and start it again (It helped in this case).
minikube delete
minikube start

c. Minikube as default uses 2048MB of memory and 2 CPUs. You can enforce Minikube to create more using command "minikube start --cpus 3 --memory 4096"
```

2. Check minikube status
```
minikube status
```

3. Use minikube cluster
```
kubectl config use-context minikube
```

4. Delete minikube cluster
```
minikube delete
```

5. Stop minikube cluster
```
minikube stop
```

6. Create and deleting resources in Minikube
```
kubectl create/delete -f deployment-app-sample.yaml
kubectl create/delete -f service-app-sample.yaml
kubectl create/delete -f permission-db-sample.yaml
kubectl create/delete -f statefulset-db-sample.yaml
kubectl create/delete -f service-db-sample.yaml

kubectl delete statefulsets <statefulset-name>
```

7. Get resource status (pods, deployments, services, statefulsets, persistentvolume, etc)
```
kubectl get pods
kubectl get deployment
kubectl get service
kubectl get statefulset
kubectl get persistentvolume
kubectl get pods <pod_name>
kubectl get persistentvolume <volume_id> --output yaml

kubectl get storageclass
```

8. Details about a pod
```
kubectl describe pods <pod_name>
kubectl describe pods | grep IP
```

9. Delete pods
```
kubectl delete pods <pod_name>
```

10. Access container
```
kubectl get pods
kubectl exec -it <pod_name> sh
```

11. Access service from url
```
minikube service <service_name>
```

12. Get url to access service
```
minikube service <service_name> --url

minikube service list

|----------------------|---------------------------|--------------|-------------------------|
|      NAMESPACE       |           NAME            | TARGET PORT  |           URL           |
|----------------------|---------------------------|--------------|-------------------------|
| default              | db                        | No node port |                         |
| default              | kubernetes                | No node port |                         |
| default              | service-sample            |           80 | http://172.17.0.2:30061 |
| kube-system          | kube-dns                  | No node port |                         |
| kubernetes-dashboard | dashboard-metrics-scraper | No node port |                         |
| kubernetes-dashboard | kubernetes-dashboard      | No node port |                         |
|----------------------|---------------------------|--------------|-------------------------|
```

13. Dashboard resource
```
minikube dashboard
minikube dashboard --url
kubectl get service <service_name> --watch
```

14. List containers
```
https://kubernetes.io/docs/tasks/access-application-cluster/list-all-running-container-images/
```

15. Test application
```
minikube service <service_name>
```

16. Access minikube
```
minikube ssh
```


# Deploy Wordpress into the Minikube cluster
```
Apply and verify
kubectl apply -k ./ or kubectl create -k ./

Delete
kubectl delete -k ./

or execute the commands below following the sequence

kubectl create/delete -f ./app/deployment-app-sample.yaml
kubectl create/delete -f ./app/service-app-sample.yaml
kubectl create/delete -f ./db/permission-db-sample.yaml
kubectl create/delete -f ./db/statefulset-db-sample.yaml
kubectl create/delete -f ./db/service-db-sample.yaml


# Connect Wordpress with Mysql parameters

Database Name: wordpress
Username: root
Password: password
Database Host: db
Table Prefix: wp_
```


# Docker and docker-compose commands
```
Used to test a docker-compose where containers work togheter to simulate the deploy in a Kubernets cluster

docker-compose up -d
docker-compose down
docker-compose logs

docker ps

docker exec -it <database_container> sh
mysql -u root
use wordpress
```
