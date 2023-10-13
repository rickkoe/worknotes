# Ansible
## Documentation and Resources
- [Ansible Site](https://ansible.com)
- [Ansible Documentation](https://docs.ansible.com)
- [Ansible Galaxy](https://galaxy.ansible.com)
- [Online YAML Tools](https://onlineyamltools.com/edit-yaml)
- [Code Beautify](https://codebeautify.ort/yaml-editor-online)

## Galaxy Collections I Care About
### IBM Storage
- [IBM Storage Virtualize Galaxy Collection](https://galaxy.ansible.com/ibm/storage_virtualize)
- [IBM Storage Virtualize on Github](https://github.com/ansible-collections/ibm.storage_virtualize)

### Brocade
- [Brocade FOS FC Collection](https://galaxy.ansible.com/brocade/fos)
- [Brocade on Github](https://github.com/brocade/ansible)

Ansible collections stored here: `/root/.ansible/collections/ansible_collections/ibm/storage_virtualize`

## Implementation
### Install Python

```
dnf install python3.9 -y  
```

### Install Ansible

```
python3 -m pip install --user ansible  
```

### Getting Started

[Sample Ansible Setup](https://docs.ansible.com/ansible/latest/tips_tricks/sample_setup.html#sample-setup)

```
mkdir /etc/ansible
mkdir /etc/ansible/playbooks
```

You can run ansible modules directly with the command:  `ansible`
You can run an ansible playbook using the command:  `ansible-playbook`

#### Hello World! Playbook Example
1. Create the emply playbook file

    ```
    cd /etc/ansible/playbooks
    vi helloworld.yml
    ```

1. Enter the following contents in the playbook YAML file

    ```
    ---
    - name: My second playbook
    hosts: localhost

    tasks:
    - name: Print Hello World!
        debug: msg="Hello World!"
    ```

1. Run the playbook

    ```
    ansible-playbook helloworld.yml
    ```