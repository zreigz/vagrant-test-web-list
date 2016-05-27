# vagrant-test-web-list

The purpose of this project is to easily fire up a VMs with a running Docker and Kubernetes.
It creates isolated enviroment to test [web-list](https://github.com/zreigz/web-list) application. This RESTful application and Radis backend run as Docker containers.


## Prerequisities


- Vagrant 1.8.1+
- virtualbox
- Ansible 1.9+

## Start instance

```bash
$ vagrant up
```

## Test web-list application

When Vagrant instance is ready you can **ssh** to VM machine.

```bash
vagrant ssh
```

Now you can check Kubernetes PODs status.

```bash
$ kubectl get pods
```

Wait until all pods are running:

```bash
[vagrant@localhost ~]$ kubectl get pods
NAME                            READY     STATUS    RESTARTS   AGE
k8s-master-10.9.8.7             4/4       Running   1          1m
k8s-proxy-10.9.8.7              1/1       Running   0          15s
redis-master-2353460263-otdbh   1/1       Running   0          1m
web-list-o1dsg                  1/1       Running   0          1m

```

Now check web-list service status to get information about IP addresses and PORTs.

```
[vagrant@localhost ~]$ kubectl describe svc web-list
Name:			web-list
Namespace:		default
Labels:			app=web-list
Selector:		app=web-list
Type:			NodePort
IP:			10.0.0.144
Port:			<unset>	80/TCP
NodePort:		<unset>	30246/TCP
Endpoints:		10.1.57.2:8080
Session Affinity:	None
No events.

```

The web-list application is accessible with IP: `10.0.0.144` on port `80` or you can use
node IP `10.9.8.7` address with port `30246`

```bash
$ curl http://10.0.0.144/list
```

The result will be empty list: `[]`

Try add something:

```bash
curl -H "Content-Type: application/json" --data '{"text":"foo"}' http://10.0.0.144/list
```

Now when you again will ask for data you should see: `[{"text":"foo"}]`


## License
Apache