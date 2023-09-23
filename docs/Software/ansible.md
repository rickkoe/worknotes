# Ansible
## Documentation and Resources
- [Ansible Site](ansible.com)
- [Ansible Documentation](docs.ansible.com)
- [Ansible Galaxy](galaxy.ansible.com)
- [Online YAML Tools](onlineyamltools.com/edit-yaml)
- [Code Beautify](codebeautify.ort/yaml-editor-online)

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

```
cd /etc/ansible/playbooks
vi helloworld.yml

---
- name: My second playbook
  hosts: localhost

  tasks:
  - name: Print Hello World!
    debug: msg="Hello World!"

<esc> wq

ansible-playbook helloworld.yml
```