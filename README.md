# Kubernetes Intro

Environment for intro-ing Kubernetes that can run PHP apps, 10 steps for later reference as well as help during the presentation/introduction.

# Prerequisites

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Vagrant](http://vagrantup.com/downloads.html)
* Google account

# Steps for Setup

## Step 1

First we need to clone this repo down:

```bash
git clone git@github.com:Johanndutoit/k8s-intro.git intro
```

If you don't have `git` (O_O), install using:

```bash
# MAC OS
brew install git

# DEBIAN/UBUNTU
apt-get install git

# WINDOWS
¯\_(ツ)_/¯
```

## Step 2

Start up the vagrant box which we will use to demo:

```bash
vagrant up
```

When finished this will have a finished and working box ready for action, to test login:

```bash
vagrant ssh
```

Switch to root:

```bash
sudo -i
```

And check if Docker and Gcloud is installed:

```bash
docker ps
gcloud
```

If Gcloud is not found run the following:

```bash
source /root/google-cloud-sdk/path.bash.inc
source /root/google-cloud-sdk/completion.bash.inc
```

## Step 3

Firstly we'll build the test docker image, which simply runs and returns the following for us:

```bash
echo "hello world, this was run inside of a container"
```

To build the image, `cd` into your intro folder and run the following:

```bash
docker build . --tag=sample:latest
```

After which you can run the docker image using:

```bash
docker run -it sample:latest
```

Which should produce:

```bash
> hello world, this was run inside of a container
```

To view all your images run:

```bash
docker images
```

## Step 4

Now build the sample PHP app that will only show the `echo phpinfo();` function for us.

```bash
cd web
docker build . --tag=php-start:latest
```

Which will build a image with PHP working that we can now start using.

## Step 5

Run the image to see it running in docker:

```bash
docker run -p 80:80 php-start:latest 
```

This should start the docker image in the foreground, and you should be able to access the app from [http://http://192.168.200.10/](http://192.168.200.10/).

To run the app in the background use:

```bash
docker run -d -p 80:80 php-start:latest
```

After running that you should be able to access the site again from [http://http://192.168.200.10/](http://192.168.200.10/), but also view a running container in:

```bash
docker ps
```

## Step 6

Next we probably want to deploy this to a Kubernetes cluster.

First we have to make sure `gcloud` is available and `kubectl` is installed:

```bash
gcloud components list
```

If `kubectl` is not installed, run:

```bash
gcloud components install kubectl
```

## Step 7

After installing `kubectl` make sure you Google Account is linked to this local instance of `gcloud`:

```bash
gcloud auth login
```

## Step 8

Head to [Container Engine](https://console.cloud.google.com/kubernetes/list) and create a cluster, once created link your local `kubectl` tool using:

```bash
gcloud container clusters get-credentials {cluster-name} --zone={zone}
```

After which commands like:

```bash
kubectl get pods
```

should work like a charm.

For a test run:

```bash
kubectl get nodes
```

To view all your nodes (servers) in your cluster.

## Step 9

Create a Replication Controller on Kubernetes with our stater php app

```bash
kubecreate create -f pod.yaml
```

Which should create 1 resource under both `RC` and `pods`:

```bash
kubectl get pods
kubectl get rc
```

## Step 10

Now to actually access your app running on Kubernetes, we will need to create a service that will expose it to the external world:

```bash
kubectl create -f service.yaml
```

Keep checking `kubect get services` where you will find the public IP of your new load balancer that can be used to access your app running on Kubernetes ;).

## Cleanup

Remember to run:

```bash
vagrant destroy
```

Which will delete the installed virtual machine we used during these steps.

# License

MIT License

Copyright (c) 2017 Johann du Toit

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.