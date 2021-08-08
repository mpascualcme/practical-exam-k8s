# kubernetes exam

* use exam.ova from our google drive: student workspace / image
* configure the vm as follows:

```
cpu: 2 vcpu
memory: 3072 Mb
networking: bridged, then select your wifi adapter under name. Disable the second network interface
```

* configuration less than these specs will fail
* ensure that there aren't too many desktop apps open/running on your laptop. Those will compete for resources with your vm
* once the system boots, install 'network-scripts' package. run 'ifconfig' and 'route -n' and save the output for reference
* change the hostname of your server to 'yourfirstname'. check that you can ping www.google.com
* edit /root/.bashrc, add:

```
HISTSIZE=1000000
HISTIGNORE="ls:ll:pwd:bg:fg:history"
HISTCONTROL=ignoreboth:erasedups
HISTTIMEFORMAT="%d/%m/%y %T "
```

* disable NetworkManager, configure bootproto: none with the appropriate settings (refer to 'ifconfig' and 'route -n' output)
* hint: configure IPADDR, NETMASK, and GATEWAY
* ensure that /etc/resolv.conf is pointing to your GATEWAY
* install kuberentes master. Follow the instructions from https://www.tecmint.com/install-a-kubernetes-cluster-on-centos-8/
* we will only use a single node for this exam. Follow the instructions for the master node, and ignore instructions for the worker nodes.
* NOTE: before you start kubelet, run this command

```
echo "KUBELET_EXTRA_ARGS=--cgroup-driver=cgroupfs" > /etc/sysconfig/kubelet
```

* before you run 'kubeadm init', install 'tc'. If 'kubeadm init' is preventing you from starting due to warnings, add '--ignore-preflight-errors=all'
* by default, kubernetes disallows running of application pods on the master. bypass ths behavior by running "k taint nodes <name of your node> node-role.kubernetes.io/master-"
* congratulations! we can now start deploying applications to your cluster
* deploy your first nginx application using below's manifest. reference: https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 1
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

* if you wish to restart the kubernetes installation, do it with;

```
kubeadm reset --force; rm -rf /etc/cni/net.d/
```

* modify the nginx deployment to spin up 3 copies of the application
* once everything is done, shut down your virtual machine, export, and upload to google drive with the file named: 'firstname.k8s.ova'.
* DEADLINE for submission: Tuesday Aug 10, 10pm 

