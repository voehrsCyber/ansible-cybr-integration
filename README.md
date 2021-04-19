# Ansible Cyberark Conjur Integration

I created an inventory under /etc/ansible/hosts called appservers. I also created a hostfactory by adding the following policy into conjur:
```
- !host-factory
    id: ansible_factory
    layers: cicd/ansible
```
After adding this policy I ran following command on the CLI to retrieve the hostfactory token to be able to create new hosts:
`conjur hostfactory tokens create ansible_factory`
The presented token needs to be stored in the ENV of the ansible machine as HFTOKEN
