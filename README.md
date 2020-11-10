# Packer Debian Repo and K8s CKA Study Lab

This rough little repo is just a store for me to keep my study lab files. I had a fully working one on another laptop but didnt have a automated way of building my images. This aims to solv that.

- Allows me to learn about packer and creating custom vagrant boxes.
  - Uses debian preseeds generated by hook and by crook. (Borrowed some bits)
- Allows me to create an environment for Kubernetes training
  - caching scripts that stores docker images on the vagrant share to speed up `kubeadm init`
    - on the vms it's `./share/vagrant-scripts/{load,save}-images`
This was originially a centos thing, but for some reason it seems I couldnt make the CNI plugins work properly on Centos. This is likely due to my own lack of training in the RHEL space.

It would seem Debian is less secure by default, but also simpler to use for learning, less gotchas.

To use:

- `make keys` to generate vagrant user keys
- `make stage1` to pull the iso into packer's cache and build the first stage
- `make stage2` to build the second phase (These are 2 stages, because it allows me to debug the builds easier.)
- `make import` to install the new box into vagrant.

The k8s bit:

- `vagrant up loadbalancer controller-0 worker-0` for a small cluster.
- `vagrant up loadbalancer controller-{0,1,2} worker-{0,1,2}` for a bigger one.
