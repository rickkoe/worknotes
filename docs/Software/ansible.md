# Ansible
## Documentation and Resources
- [Ansible Site](ansible.com)
- [Ansible Documentation](docs.ansible.com)
- [Ansible Galaxy](galaxy.ansible.com)
- [Online YAML Tools](onlineyamltools.com/edit-yaml)
- [Code Beautify](codebeautify.ort/yaml-editor-online)

## Stuff I care about
- [IBM Storage Virtualize Collection](https://github.com/ansible-collections/ibm.storage_virtualize)

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