# What happens when you place a WCP (Tanzu) enabled vCenter cluster into mainenance mode (one ESXi server) 
Tested on vCenter 7.0.3 Build 19480866

## Here is the vCenter cluster layout 

ESXi 1 and ESXi2 and ESXi3 have their WCP supervisor VM's 1,2,3 on ESXi1,2,3 (What are the chances...) 

![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/ESXi1.png)
![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/ESXi2.png)
![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/ESXi3.png)

## And logging onto the WCP API end point

```
[root@orfdns ~]# /usr/local/bin/kubectl-vsphere login --vsphere-username administrator@vsphere.local --server=https://192.168.5.61 --insecure-skip-tls-verify

KUBECTL_VSPHERE_PASSWORD environment variable is not set. Please enter the password below
Password: 
Logged in successfully.

You have access to the following contexts:
   192.168.5.61
   namespace1000

If the context you wish to use is not in this list, you may need to try
logging in again later, or contact your cluster administrator.

To change context, use `kubectl config use-context <workload name>`
[root@orfdns ~]# k1
Switched to context "namespace1000".
[root@orfdns ~]# k get nodes
NAME                               STATUS   ROLES                  AGE   VERSION
4230512d69b675b89193359b22eabd78   Ready    control-plane,master   39m   v1.21.0+vmware.wcp.2
42305dbfd81a86bffc44960b1343f25b   Ready    control-plane,master   39m   v1.21.0+vmware.wcp.2
4230a504394228d74b4bc4be7bff0a7c   Ready    control-plane,master   43m   v1.21.0+vmware.wcp.2
[root@orfdns ~]# 
```

## Now lets put ESXi3 into maintenance mode 

![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/m1.png)

Some warnings 
![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/w1.png)
Information
![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/w2.png)

## Result ESXi3 is in maintenance mode

![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/e3inMaint.png)

## Where are the sup VM's - one ESX1 and ESXi2 

![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/onesupvm.png)
![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/twosupvms.png)

## Checking if my API end point still works

```
[root@orfdns ~]# /usr/local/bin/kubectl-vsphere login --vsphere-username administrator@vsphere.local --server=https://192.168.5.61 --insecure-skip-tls-verify

KUBECTL_VSPHERE_PASSWORD environment variable is not set. Please enter the password below
Password: 
Logged in successfully.

You have access to the following contexts:
   192.168.5.61
   namespace1000

If the context you wish to use is not in this list, you may need to try
logging in again later, or contact your cluster administrator.

To change context, use `kubectl config use-context <workload name>`
[root@orfdns ~]# k1
Switched to context "namespace1000".
[root@orfdns ~]# k get nodes
NAME                               STATUS   ROLES                  AGE   VERSION
4230512d69b675b89193359b22eabd78   Ready    control-plane,master   57m   v1.21.0+vmware.wcp.2
42305dbfd81a86bffc44960b1343f25b   Ready    control-plane,master   57m   v1.21.0+vmware.wcp.2
4230a504394228d74b4bc4be7bff0a7c   Ready    control-plane,master   62m   v1.21.0+vmware.wcp.2
[root@orfdns ~]# k get pods -A
NAMESPACE                                   NAME                                                              READY   STATUS      RESTARTS   AGE
kube-system                                 coredns-689d589965-2qldl                                          1/1     Running     0          62m
kube-system                                 coredns-689d589965-4qvmg                                          1/1     Running     7          62m
kube-system                                 coredns-689d589965-qvp69                                          1/1     Running     0          62m
kube-system                                 docker-registry-4230512d69b675b89193359b22eabd78                  1/1     Running     0          56m
kube-system                                 docker-registry-42305dbfd81a86bffc44960b1343f25b                  1/1     Running     0          56m
kube-system                                 docker-registry-4230a504394228d74b4bc4be7bff0a7c                  1/1     Running     0          61m
kube-system                                 etcd-4230512d69b675b89193359b22eabd78                             1/1     Running     0          57m
kube-system                                 etcd-42305dbfd81a86bffc44960b1343f25b                             1/1     Running     0          56m
kube-system                                 etcd-4230a504394228d74b4bc4be7bff0a7c                             1/1     Running     0          61m
kube-system                                 kube-apiserver-4230512d69b675b89193359b22eabd78                   1/1     Running     1          52m
kube-system                                 kube-apiserver-42305dbfd81a86bffc44960b1343f25b                   1/1     Running     0          51m
kube-system                                 kube-apiserver-4230a504394228d74b4bc4be7bff0a7c                   1/1     Running     1          56m
kube-system                                 kube-controller-manager-4230512d69b675b89193359b22eabd78          1/1     Running     2          57m
kube-system                                 kube-controller-manager-42305dbfd81a86bffc44960b1343f25b          1/1     Running     0          56m
kube-system                                 kube-controller-manager-4230a504394228d74b4bc4be7bff0a7c          1/1     Running     3          61m
kube-system                                 kube-proxy-4rrgd                                                  1/1     Running     0          52m
kube-system                                 kube-proxy-7dtr6                                                  1/1     Running     0          53m
kube-system                                 kube-proxy-qtxlr                                                  1/1     Running     0          62m
kube-system                                 kube-scheduler-4230512d69b675b89193359b22eabd78                   2/2     Running     5          54m
kube-system                                 kube-scheduler-42305dbfd81a86bffc44960b1343f25b                   2/2     Running     3          55m
kube-system                                 kube-scheduler-4230a504394228d74b4bc4be7bff0a7c                   2/2     Running     8          62m
kube-system                                 kubectl-plugin-vsphere-4230512d69b675b89193359b22eabd78           1/1     Running     2          56m
kube-system                                 kubectl-plugin-vsphere-42305dbfd81a86bffc44960b1343f25b           1/1     Running     2          56m
kube-system                                 kubectl-plugin-vsphere-4230a504394228d74b4bc4be7bff0a7c           1/1     Running     3          62m
kube-system                                 wcp-authproxy-4230512d69b675b89193359b22eabd78                    1/1     Running     0          56m
kube-system                                 wcp-authproxy-42305dbfd81a86bffc44960b1343f25b                    1/1     Running     0          56m
kube-system                                 wcp-authproxy-4230a504394228d74b4bc4be7bff0a7c                    1/1     Running     0          59m
kube-system                                 wcp-fip-4230512d69b675b89193359b22eabd78                          1/1     Running     0          51m
kube-system                                 wcp-fip-42305dbfd81a86bffc44960b1343f25b                          1/1     Running     0          56m
kube-system                                 wcp-fip-4230a504394228d74b4bc4be7bff0a7c                          1/1     Running     0          61m
svc-tmc-c8                                  tmc-agent-installer-27598296-ggcvl                                0/1     Completed   0          43s
vmware-system-ako                           vmware-system-ako-ako-controller-manager-548f76584f-zfsvj         1/1     Running     0          62m
vmware-system-appplatform-operator-system   vmware-system-appplatform-operator-mgr-0                          1/1     Running     0          62m
vmware-system-capw                          capi-controller-manager-584fd8896-9m2ls                           2/2     Running     0          60m
vmware-system-capw                          capi-controller-manager-584fd8896-p2pdc                           2/2     Running     5          60m
vmware-system-capw                          capi-controller-manager-584fd8896-qjs6g                           2/2     Running     2          60m
vmware-system-capw                          capi-kubeadm-bootstrap-controller-manager-7c66845b75-58pdf        2/2     Running     0          60m
vmware-system-capw                          capi-kubeadm-bootstrap-controller-manager-7c66845b75-8bfzb        2/2     Running     5          60m
vmware-system-capw                          capi-kubeadm-bootstrap-controller-manager-7c66845b75-zfzf9        2/2     Running     0          60m
vmware-system-capw                          capi-kubeadm-bootstrap-webhook-85b6b6f795-n496g                   2/2     Running     2          60m
vmware-system-capw                          capi-kubeadm-bootstrap-webhook-85b6b6f795-nq5x8                   2/2     Running     3          60m
vmware-system-capw                          capi-kubeadm-bootstrap-webhook-85b6b6f795-sb242                   2/2     Running     1          60m
vmware-system-capw                          capi-kubeadm-control-plane-controller-manager-698b658fd9-5zmbc    2/2     Running     0          60m
vmware-system-capw                          capi-kubeadm-control-plane-controller-manager-698b658fd9-wv7b7    2/2     Running     5          60m
vmware-system-capw                          capi-kubeadm-control-plane-controller-manager-698b658fd9-xs4z8    2/2     Running     2          60m
vmware-system-capw                          capi-kubeadm-control-plane-webhook-54b4c9857c-lwhcm               2/2     Running     3          60m
vmware-system-capw                          capi-kubeadm-control-plane-webhook-54b4c9857c-wcwdn               2/2     Running     0          60m
vmware-system-capw                          capi-kubeadm-control-plane-webhook-54b4c9857c-xd62t               2/2     Running     2          60m
vmware-system-capw                          capi-webhook-f48c7ff58-qpw7r                                      2/2     Running     2          60m
vmware-system-capw                          capi-webhook-f48c7ff58-tmzc4                                      2/2     Running     2          60m
vmware-system-capw                          capi-webhook-f48c7ff58-xjwwx                                      2/2     Running     0          60m
vmware-system-capw                          capw-controller-manager-57974596cf-4h2cg                          2/2     Running     0          60m
vmware-system-capw                          capw-controller-manager-57974596cf-l8kf5                          2/2     Running     5          60m
vmware-system-capw                          capw-controller-manager-57974596cf-zg7jq                          2/2     Running     0          60m
vmware-system-capw                          capw-webhook-76bc4d996b-glm2r                                     2/2     Running     0          60m
vmware-system-capw                          capw-webhook-76bc4d996b-rw5cg                                     2/2     Running     0          60m
vmware-system-capw                          capw-webhook-76bc4d996b-xf9sb                                     2/2     Running     1          60m
vmware-system-cert-manager                  cert-manager-799b5bbfdf-67vlh                                     1/1     Running     1          61m
vmware-system-cert-manager                  cert-manager-cainjector-69c886766f-v78fm                          1/1     Running     5          61m
vmware-system-cert-manager                  cert-manager-webhook-657b4fdd6c-4fgg2                             1/1     Running     0          61m
vmware-system-csi                           vsphere-csi-controller-5d94c7dfc7-v8j42                           6/6     Running     19         59m
vmware-system-kubeimage                     image-controller-6b99d67685-qr56f                                 1/1     Running     1          61m
vmware-system-license-operator              vmware-system-license-operator-controller-manager-75869df98zbcr   1/1     Running     0          59m
vmware-system-license-operator              vmware-system-license-operator-controller-manager-75869df9dpq52   1/1     Running     0          59m
vmware-system-license-operator              vmware-system-license-operator-controller-manager-75869df9q4jzt   1/1     Running     4          59m
vmware-system-logging                       fluentbit-8r86f                                                   1/1     Running     0          56m
vmware-system-logging                       fluentbit-nz545                                                   1/1     Running     0          62m
vmware-system-logging                       fluentbit-rvxls                                                   1/1     Running     0          56m
vmware-system-netop                         vmware-system-netop-controller-manager-7fc9f6bf4c-hzqs5           2/2     Running     5          62m
vmware-system-netop                         vmware-system-netop-controller-manager-7fc9f6bf4c-lpj7d           2/2     Running     1          62m
vmware-system-netop                         vmware-system-netop-controller-manager-7fc9f6bf4c-phkt2           2/2     Running     0          62m
vmware-system-nsop                          vmware-system-nsop-controller-manager-65b595844b-4xpfg            1/1     Running     2          59m
vmware-system-nsop                          vmware-system-nsop-controller-manager-65b595844b-5bx9k            1/1     Running     0          59m
vmware-system-nsop                          vmware-system-nsop-controller-manager-65b595844b-9z44f            1/1     Running     6          59m
vmware-system-registry                      vmware-registry-controller-manager-5d59575b5c-5bpg9               2/2     Running     8          61m
vmware-system-tkg                           masterproxy-tkgs-plugin-bptmw                                     1/1     Running     0          52m
vmware-system-tkg                           masterproxy-tkgs-plugin-hdtpx                                     1/1     Running     0          53m
vmware-system-tkg                           masterproxy-tkgs-plugin-hrmgk                                     1/1     Running     0          59m
vmware-system-tkg                           tkgs-plugin-server-675575f5cd-fwzqz                               1/1     Running     0          59m
vmware-system-tkg                           tkgs-plugin-server-675575f5cd-gcqzc                               1/1     Running     2          59m
vmware-system-tkg                           tkgs-plugin-server-675575f5cd-hszbv                               1/1     Running     2          59m
vmware-system-tkg                           vmware-system-tkg-controller-manager-78c746fdf7-5wpnk             2/2     Running     0          60m
vmware-system-tkg                           vmware-system-tkg-controller-manager-78c746fdf7-95f6b             2/2     Running     2          60m
vmware-system-tkg                           vmware-system-tkg-controller-manager-78c746fdf7-jqbrg             2/2     Running     5          60m
vmware-system-tkg                           vmware-system-tkg-webhook-c9ff5c794-6lxrm                         2/2     Running     0          60m
vmware-system-tkg                           vmware-system-tkg-webhook-c9ff5c794-plvmj                         2/2     Running     0          60m
vmware-system-tkg                           vmware-system-tkg-webhook-c9ff5c794-zdg68                         2/2     Running     2          60m
vmware-system-ucs                           upgrade-compatibility-service-84fb4f558b-l8fqk                    1/1     Running     0          61m
vmware-system-ucs                           upgrade-compatibility-service-84fb4f558b-m4nqx                    1/1     Running     0          61m
vmware-system-ucs                           upgrade-compatibility-service-84fb4f558b-v4tmp                    1/1     Running     0          61m
vmware-system-vmop                          vmware-system-vmop-controller-manager-79fb54ffdf-bdq2w            2/2     Running     0          60m
vmware-system-vmop                          vmware-system-vmop-controller-manager-79fb54ffdf-gp64x            2/2     Running     4          60m
vmware-system-vmop                          vmware-system-vmop-controller-manager-79fb54ffdf-n8xh4            2/2     Running     0          60m
[root@orfdns ~]#
```

# Removing the maintenance mode on the ESXi server

![Version](https://github.com/ogelbric/MaintenanceModeWCP_enabled/blob/main/rem_maint.png)



