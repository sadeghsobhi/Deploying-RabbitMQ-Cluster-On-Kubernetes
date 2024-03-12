# https://www.linkedin.com/pulse/deploying-rabbitmq-cluster-kubernetes-part-1-darshana-dinushal


# Deploying RabbitMQ Cluster On Kubernetes

## Deploy RabbitMQ Cluster
In here I will put every command that can run on both Kubernetes (> kubectl command) and openshift (> oc command ) .If your using OpenShift run the openshift command.

## Step 01 Create Deployment

1.) Create "rabbit-configmap.yaml" using vi command.(Link)
Execute the following command to create configMap.

```
kubectl create -n rabbits -f rabbit-configmap.yaml
```
2.) Create Role-based access control (RBAC)

Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization.
Execute the following command to create RBAC.
```
kubectl create -n rabbits -f rabbit-rbac.yaml
```
3.) Create Secret for the Erlang Cooke.

RabbitMQ nodes and CLI tools use a cookie to determine whether they are allowed to communicate with each other. For two nodes to be able to communicate they must have the same shared secret called the Erlang cookie. 
Execute the following command to create Erlang cookie secret.
```
kubectl create -n rabbits -f rabbit-secret.yaml
```
4.) Create StatefulSet for RabbitMQ

RabbitMQ requires using a Stateful Set to deploy a RabbitMQ cluster to Kubernetes. The Stateful Set ensures that the RabbitMQ nodes are deployed in order, one at a time. There are other, equally important reasons for using a Stateful Set instead of a Deployment: sticky identity, simple network identifiers, stable persistent storage and the ability to perform ordered rolling upgrades.

By using a StatefulSet as is intended for the case where Pods have persistent data that is associated with their identity. If the RabbitMQ cluster down ,pods restart or pods failure Kubernetes will create new pods ,but having persistent storage, rabbitmq queue messages will not delete.
Replace the storageClassName in the "rabbit-statefulset.yaml".
```
kubectl get storageclass
```

![Screenshot from 2024-03-12 15-21-33](https://github.com/sadeghsobhi/Deploying-RabbitMQ-Cluster-On-Kubernetes/assets/108259940/c4e255ff-1005-493b-8d74-9ed32644ba6e)






