# Ansible Cyberark Conjur Integration

To start the whole integration I integrated the conjur collection from galaxy:
```
ansible-galaxy collection install cyberark.conjur
```
I created an inventory under /etc/ansible/hosts called appservers. 
I also created a hostfactory by adding the following policy into conjur:
```
- !host-factory
    id: ansible_factory
    layers: cicd/ansible
```
The layer already exist and was granted to retrieve existing secrets. This can be adjusted to the existing layers.

After adding this policy I ran following command on the CLI to retrieve the hostfactory token to be able to create new hosts:
`conjur hostfactory tokens create ansible_factory`
The presented token needs to be stored in the ENV of the ansible machine as HFTOKEN.
The host of the inventory needs internet access to retrieve Summon and Summon-Conjur.
After adding all the details required the host can retrieve secrets via summon.

The second part of the playbook uses the ansible host to retrieve the secrets from conjur. Therefore the ansible host needs all details mentioned in the documentation. To make the life easier I used the following command in the CLI `conjur hostfactory hosts create "HOSTFACTORY TOKEN FROM THE BEVOR STEP" ansible-localhost`.

Current limitations:
- The role "conjur_host_identity" requires the hosts to have internet access. 
- The mentioned role in the documentation has to be "_" and not "-"
- The playbook in the documentation uses ",", which caused issues in my test. I removed them.
