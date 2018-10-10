### This is a repo for setting up a ipv6 only k8s cluster in vagrant.

Design considerations:

* The automount of the $CWD to /vagrant has been disabled.
* /shared is rsynced unidirectionally to /shared on each node.
* The hostnames defined in the Vagrantfile are resolvable within the cluster.
* You can use Environment Variables or direnv to set the number of ETCD, Master and Nodes you want to bring up in the cluster.
  * The defaults are 1 each.

- [ ] Add a script that will download the latest binaries to the /shared dir before starting the nodes.
- [ ] Add some example kubeadm configs to bring up the cluster (targeting v1.11.3 currently)