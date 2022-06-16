# monitor-ztp-sno-progress

This repo includes the files to monitor for ZTP workflow progress in the managed cluster custom resource (CR).

1. Whenever a cluster is first added to Advanced cluster management (ACM) the managed cluster CR will be given the `ztp-running` label. This signifies the cluster site-config has properly deployed via assisted service.

2. Whenever the DU profile has been successfully applied via ACM, the `ztp-done` label will be applied. This means the SNO is prepared and ready for application deployment. 

The labels are not removed, but overwritten with "true". 

```
-> oc logs -f mc-monitor-568844fbcb-d7rdb 
+ set -o errexit
+ set -o nounset
+ set -o pipefail
+ cat
+ chmod u+x /tmp/mc-monitor
+ exec oc observe managedcluster --quiet --type-env-var=KUBE_ACTION --maximum-errors=1 --resync-period=10m '--template={.metadata.name}' -- /tmp/mc-monitor

Adding true to ztp-running label on zt-sno4 - The DU profile has been applied
managedcluster.cluster.open-cluster-management.io/zt-sno4 labeled
```
