Building a single node Kubeless
-----------------------------------------------------------------------

Tested with Ansible 2.8 and an Ubuntu 18.04 VM with password-less ssh.

This is a dead simple Kubeless installation on a single node Kubernetes cluster.
The main purpose was creating a cheap function-as-a-service provider for my hobby
serverless applications. The monthly fee for a VM with a static IP is way cheaper than the monthly fee on AWS for using an Internet Gateway.

We will install everything on the remote VM and copy back the k8s config file for configuring our local `kubectl`. The stack can be deployed using the following
command:

    ansible-playbook -i hosts everything.yml

Once done, you can check the results by checking the cluster status:

    $ kubectl cluster-info

You should see the IP of your VM along with a message that Kubernetes and KubeDNS are running on it.

You can then proceed and check the status of your Kubeless installation:

    $ kubeless get-server-config

You should see the supported runtimes.

Finally you can proceed with a serverless deploy:

    $ yarn global add serverless
    $ serverless create --template kubeless-nodejs --path new-project
    $ cd new-project
    $ yarn install
    $ sls deploy

List and call your function:

	$ kubeless function ls
    $ kubeless function call capitalize --data awesome!

The above commands would need `kubectl` and `kubeless` cli tools installed locally. Moreover you would need to copy the cluster config file:

    $ mkdir -p $HOME/.kube
  	$ scp root@YOUR_HOST:/etc/kubernetes/admin.conf $HOME/.kube/config
  	$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

References:
- [Quickstart for Calico on Kubernetes](https://docs.projectcalico.org/v3.7/getting-started/kubernetes/)
- [Kubeadm Installation](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- [cgroup drivers](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
- [Kubeless Installation](https://kubeless.io/docs/quick-start/)

Well, I guess that was it!
