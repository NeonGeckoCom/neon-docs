# Install and Deploy NeonAI Diana 

Device Independent API for Neon Applications (Diana) is a collection of microservices that add functionality to NeonAI systems. Diana microservices are deployed in a Kubernetes cluster.

After you have installed Diana, use `diana --help` to get detailed help.

For more information about using NeonAI, see [the NeonAI documentation site](https://neongeckocom.github.io/neon-docs/).

## Prerequisites

To install Diana:

* [Ubuntu](https://ubuntu.com/) 20.04 or later. 
  * Diana will most likely work on other flavors of Linux, but we have not yet verified this.
* [Python](https://www.python.org/getit/) 3.8 or later.
* [Pip](https://pypi.org/project/pip/) Python package installer.

To deploy Diana: 

* [Kubectl](https://kubernetes.io/docs/reference/kubectl/) Kubernetes command-line tool.
* [Helm](https://helm.sh/) package manager for Kubernetes.
* A [Kubernetes](https://kubernetes.io/) installation.
  * The following instructions assume a local installation using [Microk8s](https://microk8s.io/) version 1.26/stable or later.
  * You can likely deploy Diana on a Kubernetes cluster in the cloud, but we have not yet verified this.

## Use a Python virtual environment

We recommend you use a [Python virtual environment](https://docs.python.org/3/library/venv.html) for installing and testing Diana. In addition to the usual benefits, using a virtual environment makes it easier to ensure you are using the correct versions of Python and Diana.

1. Create a Python virtual environment:

```
python3.10 -m venv venv
```

2. Open a new terminal window. Activate the Python virtual environment in this new window:

```
. venv/bin/activate
```

Using this new window, proceed with the instructions.

## Install Diana

3. Use Pip to install Diana:

```
pip install --pre neon-diana-utils
```

This command installs the newest pre-release version, which is described in this tutorial. 

**Warning:** You can use `pip install neon-diana-utils` to install the current stable version. Version 1.0.0 includes Helm chart support. For information on installing and running older versions, see [the archived documentation in the README file](https://github.com/NeonGeckoCom/neon-diana-utils/blob/dev/README.md).

For more information on the available versions of Diana, see [the Python Package Index repo for Neon Diana](https://pypi.org/project/neon-diana-utils/).

4. Verify Diana is installed:

```
diana --version
```

**Tip:** If your computer does not recognize this command, you may need to add `~/.local/bin` to your $PATH with a command like `export PATH=$PATH:/home/${USER}/.local/bin`.

Use `diana --help` for detailed information about Diana and its commands.

The output of `diana --help` looks like:

```
Usage: diana [OPTIONS] COMMAND [ARGS]...

  Diana: Device Independent API for Neon Applications.

  See also: diana COMMAND --help

Options:
  -v, --version  Print the current version
  --help         Show this message and exit.

Commands:
  configure-backend     Configure a Diana Backend
  configure-mq-backend  Configure RabbitMQ and export user credentials
  make-github-secret    Generate Kubernetes secret for Github images
  make-keys-config      Generate a configuration file with access keys
  make-rmq-config       Generate RabbitMQ definitions
  start-backend         Start a Diana Backend
  stop-backend          Stop a Diana Backend

```

## Deploy NeonAI Diana

This example deploys Diana in a local [Microk8s Kubernetes](https://microk8s.io/) cluster. 

### Install and Run Microk8s

If you don't have Microk8s installed, you can install it and create the necessary user with:

```
sudo snap install microk8s --classic
sudo usermod -aG microk8s $USER
newgrp microk8s
```

5. Start Microk8s:

```
microk8s start
```

6. Enable the services for persistent storage, DNS, and the Kubernetes dashboard:

```
microk8s enable hostpath-storage
microk8s enable dns
microk8s enable dashboard
```

7. Enable the MetalLB service:

```
microk8s enable metallb
```

At the prompt, enter a subnet  which is not being used by your router. For example: 

```
10.10.10.10-10.10.10.10
```

Note: Unless you plan on adding multiple nodes, this range only needs one address.

8. After Microk8s is running, use `microk8s kubectl create token default` to create a Microk8s token.

9. In the new terminal window, use `microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 1443:443` to forward the dashboard port. 

You can now access your Kubernetes dashboard in a browser at https://localhost:1443/ using the token you created in step 2. 

10. The process in this terminal needs to keep running. Either background the process, or leave this terminal window open and open a new terminal window to continue working.

### Set Up DNS

The ingress controller needs URLs to be mapped to services. There are a number of different ways you can accomplish this, depending on your networking setup. 

For this guide, we will use the simple case of editing the `/etc/hosts` file.

11. Edit the `/etc/hosts` file. Add one entry for the domain name of each service you intend to run. Add the canonical domain and point it to the IP address you gave MetalLB in step 3.

For example, if you plan to run a service named `test-service` on the `diana.k8s` domain, add the following line:

```
10.10.10.10 test-service.diana.k8s
```

This tells your computer that `test-service.diana.k8s` is at IP address `10.10.10.10`. Your computer will route all requests for `test-service.diana.k8s` to the Kubernetes cluster you set up at `10.10.10.10`, instead of looking for it on the public internet.

Add one line for each service. Point each service to the same IP address.


### Prepare for Deployment

12. After you set up your Kubernetes cluster and configure DNS, configure the Diana backend with:

```
diana configure-mq-backend OUTPUT_PATH
```

Replace `OUTPUT_PATH` with the directory where you want Diana to store its Helm charts and configurations. For example:

```
diana configure-mq-backend ~/neon_diana
```

Follow the prompts to provide any necessary configuration parameters. 

13. **Optional:** To add extra TCP ports (i.e. for RabbitMQ), update the `OUTPUT_PATH/ingress-nginx/values.yaml` file accordingly.

14. Deploy the NGINX ingress:

```
helm install ingress-nginx OUTPUT_PATH/ingress-common --namespace ingress-nginx --create-namespace
```

15. Edit `OUTPUT_PATH/diana-backend-values.yaml` and update any necessary configuration. At minimum, you need to update the following parameters:

* `domain` Change this to the domain you added to the `/etc/hosts` file in step 11.
* `letsencrypt.email` If you are using a "real" domain, change this to the email address you want to use for the [Let's Encrypt](https://letsencrypt.org/) SSL certificate. For local testing, leave this as is.
* `letsencrypt.server` If you are using a "real" domain, change this to a valid Let's Encrypt server address, such as `https://acme-v02.api.letsencrypt.org/directory`. For local testing, leave this as is.

## Deploy the Diana Backend

16. Update the Helm dependency:

```
helm dependency update OUTPUT_PATH/diana-backend
```

17. Use Helm to launch Diana:

```
helm install diana-backend OUTPUT_PATH/test/diana-backend --namespace backend --create-namespace
```

This creates the `backend` namespace and launches Diana into that namespace. You can change this to any namespace name you prefer. You may want to use separate namespaces for test versus production deployments, to separate the Diana backend from other deployments, or both.

```
