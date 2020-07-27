# pvc-io-test

Crude test of I/O performance for different storage classes by creating PersistVolumeClaims

## Update charts/pvctest/values.yaml with the list of available storageClasses

## Deploy helm chart
- Will create a PersistVolumeClaims for each of the items in the values.yaml file
- Will create a pod for that mounts each different PersistVolumeClaims
	- Each pod will run a dd command that will write some data and measure how long it takes

`helm upgrade --install pvctest charts/pvctest`

## After pods have completed, pull details from pod logs (This section needs to be implemented better)

- Get list of pods
`oc get pods | grep -v NAME | awk '{print $1}' > pod-list.txt`

- For each pod fetch its logs to get performance info from dd
`cat pod-list.txt | 2>&1 xargs -n 1 -I mystring sh -c "echo \"******************\" ; echo mystring; echo \"--------------\"; oc logs mystring ; echo \" \""`

- The output will looking something like

```
******************
pvc-test-portworx-db-gp3-lpznb
--------------
1000000000 bytes (1.0 GB, 954 MiB) copied, 4 s, 230 MB/s
4+0 records in
4+0 records out
1000000000 bytes (1.0 GB, 954 MiB) copied, 4.34163 s, 230 MB/s
```
