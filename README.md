# OpenShift Host Preparation - Ansible Playbook

This repository is an Ansible Playbook that prepares hosts for an OpenShift
installation.

It uses the same inventory file as the `openshift-ansible` playbooks.

## Usage

```
ansible-playbook -i /etc/ansible/hosts prepare.yml
```
