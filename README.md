# openshift-scaling


## Add Nodes

### Create MachineSet

    CLUSTERID=$(oc get machineset -n openshift-machine-api -o jsonpath='{.items[0].metadata.labels.machine\.openshift\.io/cluster-api-cluster}')
    echo $CLUSTERID

    curl -s https://raw.githubusercontent.com/luigiaparicio/openshift-scaling/main/cluster-config/elastic-machineset.yaml | sed -e "s/CLUSTERID/${CLUSTERID}/g" | oc apply -f -


## Cluster and Machine autoscaling

### Create Cluster Autoscaler

    oc apply -f https://raw.githubusercontent.com/luigiaparicio/openshift-scaling/main/cluster-config/clusterautoscaler.yaml


### Create Machine Autoscaler

    CLUSTERID=$(oc get machineset -n openshift-machine-api -o jsonpath='{.items[0].metadata.labels.machine\.openshift\.io/cluster-api-cluster}')
    echo $CLUSTERID

    curl -s https://raw.githubusercontent.com/luigiaparicio/openshift-scaling/main/cluster-config/elastic-machineautoscaler.yaml | sed -e "s/CLUSTERID/${CLUSTERID}/g" | oc apply -f -


### Kubelet Config for 500 MaxPods

    oc apply -f https://raw.githubusercontent.com/luigiaparicio/openshift-scaling/main/cluster-config/kubeletconfig-large-pods.yaml
