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
+ exec oc observe managedcluster --type-env-var=KUBE_ACTION --maximum-errors=1 --resync-period=10m '--template={.metadata.name}' -- /tmp/mc-monitor
# 2022-06-16T15:51:08Z Sync started
# 2022-06-16T15:51:08Z Sync 302664577	/tmp/mc-monitor local-cluster local-cluster

# 2022-06-16T15:51:08Z Sync 302664626	/tmp/mc-monitor zt-sno3 zt-sno3

# 2022-06-16T15:51:08Z Sync 302669066	/tmp/mc-monitor zt-sno4 zt-sno4

# 2022-06-16T15:51:18Z Sync ended
# 2022-06-16T15:51:18Z Updated 302699798	/tmp/mc-monitor zt-sno4 zt-sno4

Adding true to ztp-running label on zt-sno4 - The DU profile has been applied
managedcluster.cluster.open-cluster-management.io/zt-sno4 labeled
# 2022-06-16T15:51:31Z Updated 302700180	/tmp/mc-monitor zt-sno4 zt-sno4

No resources found
No resources found
```
