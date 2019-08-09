## setup a cicd system in ACK kubernetes cluster

### 1. add helm repo

check if jenkins helm chart is available:

```
$ helm repo list
NAME     	URL
stable   	http://aliacs-k8s-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/app/charts/
local    	http://127.0.0.1:8879/charts
incubator	http://aliacs-k8s-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/app/charts-incubator/

$ helm search jenkins
NAME                     	CHART VERSION	APP VERSION	DESCRIPTION
stable/jenkins           	0.9.0        	2.181      	Open source continuous integration server. It supports mu...
```

note that we should use the jenkins helm chart version which is from `http://aliacs-k8s-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/app/charts/`, if your kubernetes cluster has not add this repo, you can add it using command like below:

```
$ helm repo add stable http://aliacs-k8s-cn-hangzhou.oss-cn-hangzhou-internal.aliyuncs.com/app/charts/

$ helm repo update
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "incubator" chart repository
...Successfully got an update from the "test" chart repository
...Successfully got an update from the "stable" chart repository
Update Complete.
```

### 2. install jenkins helm chart

get `values.yaml` template:
```
$ curl -LO https://raw.githubusercontent.com/haoshuwei/ack-jenkins/master/values.yaml
```

update `values.yaml` before install jenkins, such as the `AdminPassword` key.

then install jenkins using command like below:
```
$ helm install --namespace jenkins --values=values.yaml --name jenkins stable/jenkins
```

check if jenkins work well:
```
$ kubectl -n jenkins get po
NAME                               READY   STATUS    RESTARTS   AGE
jenkins-jenkins-5fdcb4fcdb-rpdsq   1/1     Running   0          28m
```

get LoadBalancer endpoint:
```
$ kubectl -n jenkins get svc
NAME                    TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)          AGE
jenkins-jenkins         LoadBalancer   172.21.7.65    xxx.xx.xxx.xxx   8080:31431/TCP   29m
jenkins-jenkins-agent   ClusterIP      172.21.2.109   <none>           50000/TCP        29m
```

access jenkins service:
```
http://xxx.xx.xxx.xxx:8080
```


### 3. run a pipeline

demo can be found at [Deploy Jenkins in a Kubernetes cluster and perform a pipeline build](https://www.alibabacloud.com/help/doc-detail/106712.htm)


