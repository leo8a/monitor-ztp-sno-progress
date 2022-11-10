# Launch Ansible job upon ztp-sno label

This repo includes the files to monitor for ZTP workflow progress in the managed cluster custom resource (CR).

1. After a cluster (deployed by Telco ZTP) receives the `ztp-done` label, we are now in a good spot to trigger any 
external provisioning using Automation Controller (or Tower).

2. This work reads the Automation Controller (or Tower) info from a secret (sample below), and launches a job template right 
after the `ztp-done` is applied to a managed cluster.

### To install: 

First fill in the [ztp-monitor-secret.yaml](zt-monitor-secret.yaml) with the proper info to access your Automation Controller / Tower instance.

```shell
git clone -b launch_ansible_job https://github.com/leo8a/monitor-ztp-sno-progress/
cd monitor-ztp-sno-progress
vim ztp-monitor-secret.yaml      # -> update .data.token and .data.host accordingly
```

Then, install all the required objects

```shell
cd monitor-ztp-sno-progress
oc create -k .
```

The logs should look like this:

```shell
-> oc -n ztp-day2-automation logs deploy/ztp-launch-ansible-job
+ set -o errexit
+ set -o nounset
+ set -o pipefail
+ cat
+ chmod u+x /tmp/launch_ansible_job
+ exec oc observe managedcluster --maximum-errors=1 --resync-period=10m --selector=ztp-done= -- /tmp/launch_ansible_job
# 2022-11-09T17:38:58Z Sync started
# 2022-11-09T17:38:58Z Sync 25058912	/tmp/launch_ansible_job zt-sno4 ""

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3322  100  3274  100    48   8593    125 --:--:-- --:--:-- --:--:--  8719
{"job":348,"ignored_fields":{},"id":348,"type":"job","url":"/api/v2/jobs/348/","related":{"created_by":"/api/v2/users/1/","modified_by":"/api/v2/users/1/","labels":"/api/v2/jobs/348/labels/","inventory":"/api/v2/inventories/4/","project":"/api/v2/projects/14/","organization":"/api/v2/organizations/1/","credentials":"/api/v2/jobs/348/credentials/","unified_job_template":"/api/v2/job_templates/16/","stdout":"/api/v2/jobs/348/stdout/","job_events":"/api/v2/jobs/348/job_events/","job_host_summaries":"/api/v2/jobs/348/job_host_summaries/","activity_stream":"/api/v2/jobs/348/activity_stream/","notifications":"/api/v2/jobs/348/notifications/","create_schedule":"/api/v2/jobs/348/create_schedule/","job_template":"/api/v2/job_templates/16/","cancel":"/api/v2/jobs/348/cancel/","relaunch":"/api/v2/jobs/348/relaunch/"},"summary_fields":{"organization":{"id":1,"name":"Default","description":""},"inventory":{"id":4,"name":"hub-acm-inventory","description":"","has_active_failures":false,"total_hosts":4,"hosts_with_active_failures":0,"total_groups":8,"has_inventory_sources":true,"total_inventory_sources":1,"inventory_sources_with_failures":0,"organization_id":1,"kind":""},"project":{"id":14,"name":"aap-for-ran-ztp-project","description":"","status":"successful","scm_type":"git","allow_override":false},"job_template":{"id":16,"name":"ztp-day2-automation-template","description":""},"unified_job_template":{"id":16,"name":"ztp-day2-automation-template","description":"","unified_job_type":"job"},"created_by":{"id":1,"username":"admin","first_name":"","last_name":""},"modified_by":{"id":1,"username":"admin","first_name":"","last_name":""},"user_capabilities":{"delete":true,"start":true},"labels":{"count":0,"results":[]},"credentials":[{"id":9,"name":"hub-acm-kubeconfig","description":"","kind":null,"cloud":true}]},"created":"2022-11-09T17:39:00.358504Z","modified":"2022-11-09T17:39:00.400169Z","name":"ztp-day2-automation-template","description":"","job_type":"run","inventory":4,"project":14,"playbook":"playbooks/ztp-day2-automation-labeled.yml","scm_branch":"","forks":0,"limit":"","verbosity":0,"extra_vars":"{\"state\": \"present\", \"namespace\": \"ztp-day2-automation\", \"git_protocol\": \"http\", \"git_repo_url\": \"192.168.13.11:3000/kni/aap-for-ztp-bos2.git\", \"git_scm_version\": \"master\", \"git_user\": \"kni\", \"git_password\": \"*****\", \"vault_id\": \"ztp\", \"vault_password\": \"*****\", \"target_clusters\": [\"zt-sno4\"]}","job_tags":"","force_handlers":false,"skip_tags":"","start_at_task":"","timeout":0,"use_fact_cache":false,"organization":1,"unified_job_template":16,"launch_type":"manual","status":"pending","execution_environment":null,"failed":false,"started":null,"finished":null,"canceled_on":null,"elapsed":0.0,"job_args":"","job_cwd":"","job_env":{},"job_explanation":"","execution_node":"","controller_node":"","result_traceback":"","event_processing_finished":false,"launched_by":{"id":1,"name":"admin","type":"user","url":"/api/v2/users/1/"},"work_unit_id":null,"job_template":16,"passwords_needed_to_start":[],"allow_simultaneous":false,"artifacts":{},"scm_revision":"","instance_group":null,"diff_mode":false,"job_slice_number":0,"job_slice_count":1,"webhook_service":"","webhook_credential":null,"webhook_guid":""}
Launching job template called ztp-day2-automation-template in Automation Controller/Tower for zt-sno4


# 2022-11-09T17:39:00Z Sync 25059995	/tmp/launch_ansible_job zt-sno1 ""

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
{"job":351,"ignored_fields":{},"id":351,"type":"job","url":"/api/v2/jobs/351/","related":{"created_by":"/api/v2/users/1/","modified_by":"/api/v2/users/1/","labels":"/api/v2/jobs/351/labels/","inventory":"/api/v2/inventories/4/","project":"/api/v2/projects/14/","organization":"/api/v2/organizations/1/","credentials":"/api/v2/jobs/351/credentials/","unified_job_template":"/api/v2/job_templates/16/","stdout":"/api/v2/jobs/351/stdout/","job_events":"/api/v2/jobs/351/job_events/","job_host_summaries":"/api/v2/jobs/351/job_host_summaries/","activity_stream":"/api/v2/jobs/351/activity_stream/","notifications":"/api/v2/jobs/351/notifications/","create_schedule":"/api/v2/jobs/351/create_schedule/","job_template":"/api/v2/job_templates/16/","cancel":"/api/v2/jobs/351/cancel/","relaunch":"/api/v2/jobs/351/relaunch/"},"summary_fields":{"organization":{"id":1,"name":"Default","description":""},"inventory":{"id":4,"name":"hub-acm-inventory","description":"","has_active_failures":false,"total_hosts":4,"hosts_with_active_failures":0,"total_groups":8,"has_inventory_sources":true,"total_inventory_sources":1,"inventory_sources_with_failures":0,"organization_id":1,"kind":""},"project":{"id":14,"name":"aap-for-ran-ztp-project","description":"","status":"running","scm_type":"git","allow_override":false},"job_template":{"id":16,"name":"ztp-day2-automation-template","description":""},"unified_job_template":{"id":16,"name":"ztp-day2-automation-template","description":"","unified_job_type":"job"},"created_by":{"id":1,"username":"admin","first_name":"","last_name":""},"modified_by":{"id":1,"username":"admin","first_name":"","last_name":""},"user_capabilities":{"delete":true,"start":true},"labels":{"count":0,"results":[]},"credentials":[{"id":9,"name":"hub-acm-kubeconfig","description":"","kind":null,"cloud":true}]},"created":"2022-11-09T17:39:02.395323Z","modified":"2022-11-09T17:39:02.448628Z","name":"ztp-day2-automation-template","description":"","job_type":"run","inventory":4,"project":14,"playbook":"playbooks/ztp-day2-automation-labeled.yml","scm_branch":"","forks":0,"limit":"","verbosity":0,"extra_vars":"{\"state\": \"present\", \"namespace\": \"ztp-day2-automation\", \"git_protocol\": \"http\", \"git_repo_url\": \"192.168.13.11:3000/kni/aap-for-ztp-bos2.git\", \"git_scm_version\": \"master\", \"git_user\": \"kni\", \"git_password\": \"*****\", \"vault_id\": \"ztp\", \"vault_password\": \"*****\", \"target_clusters\": [\"zt-sno1\"]}","job_tags":"","force_handlers":false,"skip_tags":"","start_at_task":"","timeout":0,"use_fact_cache":false,"organization":1,"unified_job_template":16,"launch_type":"manual","status":"pending","execution_environment":null,"failed":false,"started":null,"finished":null,"canceled_on":null,"elapsed":0.0,"job_args":"","job_cwd":"","job_env":{},"job_explanation":"","execution_node":"","controller_node":"","result_traceback":"","event_processing_finished":false,"launched_by":{"id":1,"name":"admin","type":"user","url":"/api/v2/users/1/"},"work_unit_id":null,"job_template":16,"passwords_needed_to_start":[],"allow_simultaneous":false,"artifacts":{},"scm_revision":"","instance_group":null,"diff_mode":false,"job_slice_number":0,"job_slice_count":1,"webhook_service":"","webhook_credential":null,"100  3319  100  3271  100    48   8281    121 --:--:-- --:--:-- --:--:--  8402
Launching job template called ztp-day2-automation-template in Automation Controller/Tower for zt-sno1
```

Based on work from this gist:
https://gist.github.com/bbeaudoin/6ee37424a2a4aaa42fff1bc3232426fa

Thanks @[bbeaudoin](https://gist.github.com/bbeaudoin)!
