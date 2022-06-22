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



