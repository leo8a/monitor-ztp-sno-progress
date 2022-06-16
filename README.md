# monitor-ztp-sno-progress

This repo includes the files to monitor for ZTP workflow progress in the managed cluster custom resource (CR).

1. Whenever a cluster is first added to Advanced cluster management (ACM) the managed cluster CR will be given the `ztp-running` label. This signifies the cluster site-config has properly deployed via assisted service.

2. Whenever the DU profile has been successfully applied via ACM, the `ztp-done` label will be applied. This means the SNO is prepared and ready for application deployment. 

The labels are not removed, but overwritten with "true". 
