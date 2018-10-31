# OpenShift Host Preparation - Ansible Playbook

This repository is an Ansible Playbook that prepares hosts for an OpenShift
installation.

It checks that the pre-requisites given in the OpenShift Documentation, sections [Prerequisites](https://docs.openshift.com/container-platform/latest/install_config/install/prerequisites.html)
and [Host Preparation](https://docs.openshift.com/container-platform/latest/install_config/install/host_preparation.html)
are met.

It uses the same inventory file as the `openshift-ansible` playbooks.

## Usage

If not already done, craft your inventory file as described in the
[OpenShift documentation](https://docs.openshift.com/container-platform/latest/install_config/install/advanced_install.html).

A very minimal inventory file would be:

```ini
[OSEv3]
machine1.mydomain.com
machine2.mydomain.com
 ...
machineX.mydomain.com

[OSEv3:vars]
ansible_user=john
openshift_master_default_subdomain=app.mydomain.com
```

The main parts of the inventory are:

- the `OSEv3` group where you can declare all the machines of your cluster
- the `ansible_user` variable that hold the username used to connect to the machines using SSH
- the `openshift_master_default_subdomain` is your Wildcard DNS as explained [here](https://docs.openshift.com/container-platform/3.9/install_config/install/prerequisites.html#wildcard-dns-prereq)

Then, assuming you saved your inventory file in `/etc/ansible/hosts`, you can run the playbook with:

```sh
ansible-playbook -i /etc/ansible/hosts prepare.yml
```

## What does this playbook

This playbook goes through the following steps:

- Make sure `net.ipv4.ip_forward` is set to `1`
- Remove any locally installed DNS server that might interfere with OpenShift
- Make sure nothing is listening on port 53
- Ensures there is no residue of a previous installation of Kubernetes
- Activate SELinux if it has been disabled
- Install basic admin tools (for a complete list, check the playbook)
- Install and enable NetworkManager
- Check if the Wildcard DNS is working
- Make sure the DNS names of the machines are consistent
- Enable the OpenShift repositories
- Install Docker
- Configure Docker for OverlayFS
- Make sure the filesystem holding `/var/lib/docker` is formatted as XFS
- Install the `openshift-ansible` package

Note: several steps require a reboot to apply the new configuration or detect
any misconfiguration, be prepared!
