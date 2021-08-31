# HW Production cluster

Устанавливаем kubernetes

```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://yum.kubernetes.io/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet-1.17.17-0 kubeadm-1.17.17-0 kubectl-1.17.17-0

sudo systemctl enable --now kubelet
```
---
```
kubeadm init --pod-network-cidr=192.168.0.0/24

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.24.2.22:6443 --token x0kui8.ctu748fu4cch02in --discovery-token-ca-cert-hash sha256:ac5547394d647d53101dcfa35fcdb8ff5dd9e272a1a72382281bb6a965b1e708
```
---
```
$ kubectl get nodes
NAME                         STATUS     ROLES    AGE     VERSION
sc-test-app01.phoenixit.ru   NotReady   master   4m18s   v1.17.17
```
---
```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

configmap/calico-config created
customresourcedefinition.apiextensions.k8s.io/bgpconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/bgppeers.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/blockaffinities.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/clusterinformations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/felixconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/globalnetworksets.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/hostendpoints.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamblocks.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamconfigs.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ipamhandles.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/ippools.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/kubecontrollersconfigurations.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networkpolicies.crd.projectcalico.org created
customresourcedefinition.apiextensions.k8s.io/networksets.crd.projectcalico.org created
clusterrole.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrolebinding.rbac.authorization.k8s.io/calico-kube-controllers created
clusterrole.rbac.authorization.k8s.io/calico-node created
clusterrolebinding.rbac.authorization.k8s.io/calico-node created
daemonset.apps/calico-node created
serviceaccount/calico-node created
deployment.apps/calico-kube-controllers created
serviceaccount/calico-kube-controllers created
poddisruptionbudget.policy/calico-kube-controllers created
```
---
```
$ kubectl get nodes
NAME                         STATUS   ROLES    AGE   VERSION
sc-test-app01.phoenixit.ru   Ready    master   10m   v1.17.17
```
---
```
$sudo kubeadm join 172.24.2.22:6443 --token x0kui8.ctu748fu4cch02in --discovery-token-ca-cert-hash sha256:ac5547394d647d53101dcfa35fcdb8ff5dd9e272a1a72382281bb6a965b1e708

W0831 01:53:17.232573   47543 join.go:346] [preflight] WARNING: JoinControlPane.controlPlane settings will be ignored when control-plane flag is not set.
[preflight] Running pre-flight checks
	[WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/
	[WARNING SystemVerification]: this Docker version is not on the list of validated versions: 20.10.8. Latest validated version: 19.03
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.17" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```
---
```
$ kubectl get nodes
NAME                         STATUS   ROLES    AGE   VERSION
sc-test-app01.phoenixit.ru   Ready    master   15m   v1.17.17
sc-test-app02.phoenixit.ru   Ready    <none>   94s   v1.17.17
sc-test-db01.phoenixit.ru    Ready    <none>   58s   v1.17.17
sc-test-db02.phoenixit.ru    Ready    <none>   52s   v1.17.17
```
---
```
$ kubectl apply -f deployment.yaml
deployment.apps/nginx-deployment created
```
---
```
sudo yum install -y kubelet-1.18.20-0 kubeadm-1.18.20-0 kubectl-1.18.20-0
```
---
```
[user@sc-test-app01 ~]$ kubectl get nodes
NAME                         STATUS   ROLES    AGE   VERSION
sc-test-app01.phoenixit.ru   Ready    master   42m   v1.17.17
sc-test-app02.phoenixit.ru   Ready    <none>   29m   v1.17.17
sc-test-db01.phoenixit.ru    Ready    <none>   28m   v1.17.17
sc-test-db02.phoenixit.ru    Ready    <none>   28m   v1.17.17
[user@sc-test-app01 ~]$ kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.20", GitCommit:"632ed300f2c34f6d6d15ca4cef3d3c7073412212", GitTreeState:"clean", BuildDate:"2021-08-19T15:44:22Z", GoVersion:"go1.16.7", Compiler:"gc", Platform:"linux/amd64"}
[user@sc-test-app01 ~]$ kubelet --version
Kubernetes v1.22.1
[user@sc-test-app01 ~]$ kubectl version
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.20", GitCommit:"632ed300f2c34f6d6d15ca4cef3d3c7073412212", GitTreeState:"clean", BuildDate:"2021-08-19T15:45:37Z", GoVersion:"go1.16.7", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.17", GitCommit:"f3abc15296f3a3f54e4ee42e830c61047b13895f", GitTreeState:"clean", BuildDate:"2021-01-13T13:13:00Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
WARNING: version difference between client (1.22) and server (1.17) exceeds the supported minor version skew of +/-1
```
---
```
$ kubectl describe pod kube-apiserver-sc-test-app01.phoenixit.ru -n kube-system
Name:                 kube-apiserver-sc-test-app01.phoenixit.ru
Namespace:            kube-system
Priority:             2000000000
Priority Class Name:  system-cluster-critical
Node:                 sc-test-app01.phoenixit.ru/172.24.2.22
Start Time:           Tue, 31 Aug 2021 01:39:51 +0300
Labels:               component=kube-apiserver
                      tier=control-plane
Annotations:          kubernetes.io/config.hash: aa2f24cbb357ace3d2896b750c8f9890
                      kubernetes.io/config.mirror: aa2f24cbb357ace3d2896b750c8f9890
                      kubernetes.io/config.seen: 2021-08-31T01:39:30.858132674+03:00
                      kubernetes.io/config.source: file
Status:               Running
IP:                   172.24.2.22
IPs:
  IP:           172.24.2.22
Controlled By:  Node/sc-test-app01.phoenixit.ru
Containers:
  kube-apiserver:
    Container ID:  docker://00bbf5d7fc972c6c6d2b34b8c714a0e77c446599342528794b79022376dbb1cf
    Image:         k8s.gcr.io/kube-apiserver:v1.17.17
    Image ID:      docker-pullable://k8s.gcr.io/kube-apiserver@sha256:71344dfb6a804ff6b2c8bf5f72b1f7941ddee1fbff7369836339a79387aa071a
    Port:          <none>
    Host Port:     <none>
    Command:
      kube-apiserver
      --advertise-address=172.24.2.22
      --allow-privileged=true
      --authorization-mode=Node,RBAC
      --client-ca-file=/etc/kubernetes/pki/ca.crt
      --enable-admission-plugins=NodeRestriction
      --enable-bootstrap-token-auth=true
      --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
      --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
      --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
      --etcd-servers=https://127.0.0.1:2379
      --insecure-port=0
      --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
      --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
      --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
      --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt
      --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key
      --requestheader-allowed-names=front-proxy-client
      --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt
      --requestheader-extra-headers-prefix=X-Remote-Extra-
      --requestheader-group-headers=X-Remote-Group
      --requestheader-username-headers=X-Remote-User
      --secure-port=6443
      --service-account-key-file=/etc/kubernetes/pki/sa.pub
      --service-cluster-ip-range=10.96.0.0/12
      --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
      --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
    State:          Running
      Started:      Tue, 31 Aug 2021 01:39:19 +0300
    Ready:          True
    Restart Count:  0
    Requests:
      cpu:        250m
    Liveness:     http-get https://172.24.2.22:6443/healthz delay=15s timeout=15s period=10s #success=1 #failure=8
    Environment:  <none>
    Mounts:
      /etc/kubernetes/pki from k8s-certs (ro)
      /etc/pki from etc-pki (ro)
      /etc/ssl/certs from ca-certs (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  ca-certs:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/ssl/certs
    HostPathType:  DirectoryOrCreate
  etc-pki:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/pki
    HostPathType:  DirectoryOrCreate
  k8s-certs:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/kubernetes/pki
    HostPathType:  DirectoryOrCreate
QoS Class:         Burstable
Node-Selectors:    <none>
Tolerations:       :NoExecute op=Exists
Events:            <none>
```
---
```
$ sudo kubeadm upgrade plan
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[preflight] Running pre-flight checks.
[upgrade] Running cluster health checks
[upgrade] Fetching available versions to upgrade to
[upgrade/versions] Cluster version: v1.17.17
[upgrade/versions] kubeadm version: v1.18.20
I0831 02:52:48.732778  113841 version.go:255] remote version is much newer: v1.22.1; falling back to: stable-1.18
[upgrade/versions] Latest stable version: v1.18.20
[upgrade/versions] Latest stable version: v1.18.20
[upgrade/versions] Latest version in the v1.17 series: v1.17.17
[upgrade/versions] Latest version in the v1.17 series: v1.17.17

Components that must be upgraded manually after you have upgraded the control plane with 'kubeadm upgrade apply':
COMPONENT   CURRENT        AVAILABLE
Kubelet     4 x v1.17.17   v1.18.20

Upgrade to the latest stable version:

COMPONENT            CURRENT    AVAILABLE
API Server           v1.17.17   v1.18.20
Controller Manager   v1.17.17   v1.18.20
Scheduler            v1.17.17   v1.18.20
Kube Proxy           v1.17.17   v1.18.20
CoreDNS              1.6.5      1.6.7
Etcd                 3.4.3      3.4.3-0

You can now apply the upgrade by executing the following command:

	kubeadm upgrade apply v1.18.20

_____________________________________________________________________
```
---
```
$ sudo kubeadm upgrade apply v1.18.20
[upgrade/config] Making sure the configuration is correct:
[upgrade/config] Reading configuration from the cluster...
[upgrade/config] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -oyaml'
[preflight] Running pre-flight checks.
[upgrade] Running cluster health checks
[upgrade/version] You have chosen to change the cluster version to "v1.18.20"
[upgrade/versions] Cluster version: v1.17.17
[upgrade/versions] kubeadm version: v1.18.20
[upgrade/confirm] Are you sure you want to proceed with the upgrade? [y/N]: y
[upgrade/prepull] Will prepull images for components [kube-apiserver kube-controller-manager kube-scheduler etcd]
[upgrade/prepull] Prepulling image for component etcd.
[upgrade/prepull] Prepulling image for component kube-apiserver.
[upgrade/prepull] Prepulling image for component kube-controller-manager.
[upgrade/prepull] Prepulling image for component kube-scheduler.
[apiclient] Found 0 Pods for label selector k8s-app=upgrade-prepull-etcd
[apiclient] Found 0 Pods for label selector k8s-app=upgrade-prepull-kube-scheduler
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-kube-apiserver
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-kube-controller-manager
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-etcd
[apiclient] Found 1 Pods for label selector k8s-app=upgrade-prepull-kube-scheduler
[upgrade/prepull] Prepulled image for component etcd.
[upgrade/prepull] Prepulled image for component kube-apiserver.
[upgrade/prepull] Prepulled image for component kube-controller-manager.
[upgrade/prepull] Prepulled image for component kube-scheduler.
[upgrade/prepull] Successfully prepulled the images for all the control plane components
[upgrade/apply] Upgrading your Static Pod-hosted control plane to version "v1.18.20"...
Static pod: kube-apiserver-sc-test-app01.phoenixit.ru hash: aa2f24cbb357ace3d2896b750c8f9890
Static pod: kube-controller-manager-sc-test-app01.phoenixit.ru hash: 5a5503678a4a9da6ce07502e612dedbd
Static pod: kube-scheduler-sc-test-app01.phoenixit.ru hash: 9003986d07d545f4a78b946740515555
[upgrade/etcd] Upgrading to TLS for etcd
[upgrade/etcd] Non fatal issue encountered during upgrade: the desired etcd version for this Kubernetes version "v1.18.20" is "3.4.3-0", but the current etcd version is "3.4.3". Won't downgrade etcd, instead just continue
[upgrade/staticpods] Writing new Static Pod manifests to "/etc/kubernetes/tmp/kubeadm-upgraded-manifests689636550"
W0831 02:54:59.467984  115379 manifests.go:225] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[upgrade/staticpods] Preparing for "kube-apiserver" upgrade
[upgrade/staticpods] Renewing apiserver certificate
[upgrade/staticpods] Renewing apiserver-kubelet-client certificate
[upgrade/staticpods] Renewing front-proxy-client certificate
[upgrade/staticpods] Renewing apiserver-etcd-client certificate
[upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-apiserver.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2021-08-31-02-54-57/kube-apiserver.yaml"
[upgrade/staticpods] Waiting for the kubelet to restart the component
[upgrade/staticpods] This might take a minute or longer depending on the component/version gap (timeout 5m0s)
Static pod: kube-apiserver-sc-test-app01.phoenixit.ru hash: aa2f24cbb357ace3d2896b750c8f9890
Static pod: kube-apiserver-sc-test-app01.phoenixit.ru hash: e19da861d44a107789df7df8b2c49003
[apiclient] Found 1 Pods for label selector component=kube-apiserver
[upgrade/staticpods] Component "kube-apiserver" upgraded successfully!
[upgrade/staticpods] Preparing for "kube-controller-manager" upgrade
[upgrade/staticpods] Renewing controller-manager.conf certificate
[upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-controller-manager.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2021-08-31-02-54-57/kube-controller-manager.yaml"
[upgrade/staticpods] Waiting for the kubelet to restart the component
[upgrade/staticpods] This might take a minute or longer depending on the component/version gap (timeout 5m0s)
Static pod: kube-controller-manager-sc-test-app01.phoenixit.ru hash: 5a5503678a4a9da6ce07502e612dedbd
Static pod: kube-controller-manager-sc-test-app01.phoenixit.ru hash: 6f5e838f6cefb7c13fa18fd1a5aeb4d9
[apiclient] Found 1 Pods for label selector component=kube-controller-manager
[upgrade/staticpods] Component "kube-controller-manager" upgraded successfully!
[upgrade/staticpods] Preparing for "kube-scheduler" upgrade
[upgrade/staticpods] Renewing scheduler.conf certificate
[upgrade/staticpods] Moved new manifest to "/etc/kubernetes/manifests/kube-scheduler.yaml" and backed up old manifest to "/etc/kubernetes/tmp/kubeadm-backup-manifests-2021-08-31-02-54-57/kube-scheduler.yaml"
[upgrade/staticpods] Waiting for the kubelet to restart the component
[upgrade/staticpods] This might take a minute or longer depending on the component/version gap (timeout 5m0s)
Static pod: kube-scheduler-sc-test-app01.phoenixit.ru hash: 9003986d07d545f4a78b946740515555
Static pod: kube-scheduler-sc-test-app01.phoenixit.ru hash: ccb53d41f09784f55ee8e1bda64aa36c
[apiclient] Found 1 Pods for label selector component=kube-scheduler
[upgrade/staticpods] Component "kube-scheduler" upgraded successfully!
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.18" in namespace kube-system with the configuration for the kubelets in the cluster
[kubelet-start] Downloading configuration for the kubelet from the "kubelet-config-1.18" ConfigMap in the kube-system namespace
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.18.20". Enjoy!

[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
```
---
```
$ kubeadm version
kubeadm version: &version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.20", GitCommit:"1f3e19b7beb1cc0110255668c4238ed63dadb7ad", GitTreeState:"clean", BuildDate:"2021-06-16T12:56:41Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}

$ kubelet --version
Kubernetes v1.18.20

$ kubectl version
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.20", GitCommit:"1f3e19b7beb1cc0110255668c4238ed63dadb7ad", GitTreeState:"clean", BuildDate:"2021-06-16T12:58:51Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.20", GitCommit:"1f3e19b7beb1cc0110255668c4238ed63dadb7ad", GitTreeState:"clean", BuildDate:"2021-06-16T12:51:17Z", GoVersion:"go1.13.15", Compiler:"gc", Platform:"linux/amd64"}


$ kubectl describe pod kube-apiserver-sc-test-app01.phoenixit.ru -n kube-system
Name:                 kube-apiserver-sc-test-app01.phoenixit.ru
Namespace:            kube-system
Priority:             2000000000
Priority Class Name:  system-cluster-critical
Node:                 sc-test-app01.phoenixit.ru/172.24.2.22
Start Time:           Tue, 31 Aug 2021 01:39:51 +0300
Labels:               component=kube-apiserver
                      tier=control-plane
Annotations:          kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 172.24.2.22:6443
                      kubernetes.io/config.hash: e19da861d44a107789df7df8b2c49003
                      kubernetes.io/config.mirror: e19da861d44a107789df7df8b2c49003
                      kubernetes.io/config.seen: 2021-08-31T02:55:01.414596484+03:00
                      kubernetes.io/config.source: file
Status:               Running
IP:                   172.24.2.22
IPs:
  IP:           172.24.2.22
Controlled By:  Node/sc-test-app01.phoenixit.ru
Containers:
  kube-apiserver:
    Container ID:  docker://79c69cad558320d807f5c22405e3a90b894ff6cccb969bb87ffae6515834990a
    Image:         k8s.gcr.io/kube-apiserver:v1.18.20
    Image ID:      docker-pullable://k8s.gcr.io/kube-apiserver@sha256:391cf23f3094f59e1ce222cb1fd0593ee73e120d4fdeb29d563bd0432d2e7c32
    Port:          <none>
    Host Port:     <none>
    Command:
      kube-apiserver
      --advertise-address=172.24.2.22
      --allow-privileged=true
      --authorization-mode=Node,RBAC
      --client-ca-file=/etc/kubernetes/pki/ca.crt
      --enable-admission-plugins=NodeRestriction
      --enable-bootstrap-token-auth=true
      --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
      --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
      --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
      --etcd-servers=https://127.0.0.1:2379
      --insecure-port=0
      --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
      --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
      --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
      --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt
      --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key
      --requestheader-allowed-names=front-proxy-client
      --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt
      --requestheader-extra-headers-prefix=X-Remote-Extra-
      --requestheader-group-headers=X-Remote-Group
      --requestheader-username-headers=X-Remote-User
      --secure-port=6443
      --service-account-key-file=/etc/kubernetes/pki/sa.pub
      --service-cluster-ip-range=10.96.0.0/12
      --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
      --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
    State:          Running
      Started:      Tue, 31 Aug 2021 02:55:09 +0300
    Ready:          True
    Restart Count:  0
    Requests:
      cpu:        250m
    Liveness:     http-get https://172.24.2.22:6443/healthz delay=15s timeout=15s period=10s #success=1 #failure=8
    Environment:  <none>
    Mounts:
      /etc/kubernetes/pki from k8s-certs (ro)
      /etc/pki from etc-pki (ro)
      /etc/ssl/certs from ca-certs (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  ca-certs:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/ssl/certs
    HostPathType:  DirectoryOrCreate
  etc-pki:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/pki
    HostPathType:  DirectoryOrCreate
  k8s-certs:
    Type:          HostPath (bare host directory volume)
    Path:          /etc/kubernetes/pki
    HostPathType:  DirectoryOrCreate
QoS Class:         Burstable
Node-Selectors:    <none>
Tolerations:       :NoExecute
Events:
  Type    Reason   Age   From     Message
  ----    ------   ----  ----     -------
  Normal  Pulled   4m8s  kubelet  Container image "k8s.gcr.io/kube-apiserver:v1.18.20" already present on machine
  Normal  Created  4m8s  kubelet  Created container kube-apiserver
  Normal  Started  4m7s  kubelet  Started container kube-apiserver

```
---
```
$ kubectl get pods --output=wide
NAME                               READY   STATUS    RESTARTS   AGE   IP              NODE                         NOMINATED NODE   READINESS GATES
nginx-deployment-c8fd555cc-5xldm   1/1     Running   0          43m   192.168.0.65    sc-test-app02.phoenixit.ru   <none>           <none>
nginx-deployment-c8fd555cc-clhvk   1/1     Running   0          43m   192.168.0.194   sc-test-db02.phoenixit.ru    <none>           <none>
nginx-deployment-c8fd555cc-kt8cr   1/1     Running   0          43m   192.168.0.193   sc-test-db02.phoenixit.ru    <none>           <none>
nginx-deployment-c8fd555cc-s6m7s   1/1     Running   0          43m   192.168.0.129   sc-test-db01.phoenixit.ru    <none>           <none>
```
---
```
$ kubectl drain sc-test-app02.phoenixit.ru --ignore-daemonsets

kubectl drain sc-test-app02.phoenixit.ru --ignore-daemonsets
node/sc-test-app02.phoenixit.ru already cordoned
WARNING: ignoring DaemonSet-managed Pods: kube-system/calico-node-ncc98, kube-system/kube-proxy-rr26m
evicting pod kube-system/coredns-66bff467f8-knn8x
evicting pod default/nginx-deployment-c8fd555cc-5xldm
pod/nginx-deployment-c8fd555cc-5xldm evicted
pod/coredns-66bff467f8-knn8x evicted
node/sc-test-app02.phoenixit.ru evicted
```
---
```
$ kubectl get nodes --output=wide
NAME                         STATUS                     ROLES    AGE   VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
sc-test-app01.phoenixit.ru   Ready                      master   88m   v1.17.17   172.24.2.22   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-app02.phoenixit.ru   Ready,SchedulingDisabled   <none>   74m   v1.17.17   172.24.2.23   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db01.phoenixit.ru    Ready                      <none>   73m   v1.17.17   172.24.2.24   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db02.phoenixit.ru    Ready                      <none>   73m   v1.17.17   172.24.2.25   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
```
---
```
$ sudo yum install -y kubelet-1.18.20-0 kubeadm-1.18.20-0 kubectl-1.18.20-0
$ sudo systemctl restart kubelet
Warning: kubelet.service changed on disk. Run 'systemctl daemon-reload' to reload units.
$ sudo systemctl daemon-reload
```
---
```
$ kubectl get nodes --output=wide
NAME                         STATUS                     ROLES    AGE   VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
sc-test-app01.phoenixit.ru   Ready                      master   93m   v1.17.17   172.24.2.22   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-app02.phoenixit.ru   Ready,SchedulingDisabled   <none>   79m   v1.18.20   172.24.2.23   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db01.phoenixit.ru    Ready                      <none>   78m   v1.17.17   172.24.2.24   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db02.phoenixit.ru    Ready                      <none>   78m   v1.17.17   172.24.2.25   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
```
---
```
$ kubectl uncordon sc-test-app02.phoenixit.ru
node/sc-test-app02.phoenixit.ru uncordoned

$ kubectl get nodes --output=wide
NAME                         STATUS   ROLES    AGE   VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
sc-test-app01.phoenixit.ru   Ready    master   94m   v1.17.17   172.24.2.22   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-app02.phoenixit.ru   Ready    <none>   80m   v1.18.20   172.24.2.23   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db01.phoenixit.ru    Ready    <none>   79m   v1.17.17   172.24.2.24   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db02.phoenixit.ru    Ready    <none>   79m   v1.17.17   172.24.2.25   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8

```
---
```
$ kubectl get nodes --output=wide
NAME                         STATUS   ROLES    AGE    VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                KERNEL-VERSION                CONTAINER-RUNTIME
sc-test-app01.phoenixit.ru   Ready    master   108m   v1.18.20   172.24.2.22   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-app02.phoenixit.ru   Ready    <none>   95m    v1.18.20   172.24.2.23   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db01.phoenixit.ru    Ready    <none>   94m    v1.18.20   172.24.2.24   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
sc-test-db02.phoenixit.ru    Ready    <none>   94m    v1.18.20   172.24.2.25   <none>        CentOS Linux 7 (Core)   3.10.0-1127.19.1.el7.x86_64   docker://20.10.8
```
---

```

# получение kubespray
git clone https://github.com/kubernetes-sigs/kubespray.git

# установка зависимостей
sudo pip install -r requirements.txt

# копирование примера конфига в отдельную директорию
cp -rfp inventory/sample inventory/mycluster
```
---
```
ssh-keygen -t rsa 
ssh-copy-id -i ~/.ssh/gcp-key <username>@<remote_host>
```
---
```
$ ansible-playbook -i inventory/mycluster/inventory.ini --become --become-user=root \
    --user=lex --key-file="~/.ssh/gcp-key" cluster.yml
```
---
```
master1:~$ mkdir -p $HOME/.kube
master1:~$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
master1:~$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
---
```
master1:~$ kubectl get nodes
NAME      STATUS   ROLES    AGE     VERSION
master    Ready    master   6m30s   v1.21.4
node1     Ready    <none>   5m40s   v1.21.4
node2     Ready    <none>   4m34s   v1.21.4
node3     Ready    <none>   4m34s   v1.21.4    
```
