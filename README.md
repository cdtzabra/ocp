# ocp

## Install

```shell
# user normal
mkdir ocp

cd ocp

# CREATE install-config.yaml file
## copy paste file on the server



# creer manifest
openshift-install --dir ./ create manifests
### modifier au besoin

# creer les ignition
openshift-install --dir ./ create ignition-configs

# EXPOSER server HTTP pour recuperer les fichier *.ign
python3 -m http.server 8080 --bind 0.0.0.0

```

>  **Apply ognoton files**

```shell
# BOOTSTRAP
sudo coreos-installer install /dev/sda --ignition-url http://192.168.1.197:8080/bootstrap.ign  --insecure-ignition

## Master
sudo coreos-installer install /dev/sda --ignition-url http://192.168.1.197:8080/master.ign  --insecure-ignition

## WORKER # wait-for Master to be ready with port 22623 opened
### https://api-int.<>.ign.enoks.fr:22623/config/worker
sudo coreos-installer install /dev/sda --ignition-url http://192.168.1.197:8080/worker.ign  --insecure-ignition
```

> **See installation progress**

```shell
openshift-install  agent wait-for bootstrap-complete --log-level debug
openshift-install  agent wait-for install-complete --log-level debug

```

> **Handle CSR**

* [Aproving the certificate signing requets](https://docs.redhat.com/fr/documentation/openshift_container_platform/4.12/html/machine_management/installation-approve-csrs_more-rhel-compute#installation-approve-csrs_more-rhel-compute)
```shell
## depuis le helper
oc get node
oc get csr
oc adm certificate approve  csr-xxx

```

## Some links

- https://docs.redhat.com/en/documentation/openshift_container_platform/4.8/html/support/troubleshooting#checking-load-balancer-configuration_troubleshooting-installations
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/nodes/working-with-nodes#adding-node-iso
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/networking/configuring-ipfailover#nw-ipfailover-environment-variables_configuring-ipfailover
- https://github.com/openshift/installer/blob/main/docs/user/customization.md
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/installing_an_on-premise_cluster_with_the_agent-based_installer/installation-config-parameters-agent#agent-configuration-parameters-optional_installation-config-parameters-agent
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.9/html/installing/deploying-installer-provisioned-clusters-on-bare-metal#configuring-the-install-config-file_ipi-install-installation-workflow
- https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html/networking/configuring-cluster-network-range#configuring-cluster-network-range
