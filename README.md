# story-ansible
Ansible playbook for setting up a Story node.

## Quickstart

- Clone the repository to your local machine:
```bash
git clone https://github.com/b-harvest/story-ansible.git
cd story-ansible 
```
- To execute the playbook, run:
```aiignore
ansible-playbook -i inventory.yml playbook.yml
```

## Inventory 
```yaml
all:
  vars:
    ansible_user: "ubuntu"
    ansible_port: 22
    # OpenSSH private key should be placed
    ansible_ssh_private_key_file: 'keypair'
    # if you want to specify the version of python, uncomment and set this field.
    ansible_python_interpreter: '/usr/bin/python3'

    # working directory
    user_dir: '/home/{{ ansible_user }}'

    geth_version: "v0.11.0"
    story_version: "v0.13.0"
    go_version: "1.22.2"

    # last state of system daemon.
    # if set to started, systemd daemons will be started.
    # more: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/systemd_service_module.html#ansible-collections-ansible-builtin-systemd-service-module
    systemd_state: "started"

    # customize yourself
    ufw_rules:
      - { ip: "1.1.1.1", port: "22", comment: "Control SSH" }
      - { ip: "", port: "26656", comment: "P2P Port" }
      - { ip: "1.1.1.1", port: "26660", comment: "Cosmos Exporter" }
      - { ip: "1.1.1.1", port: "59100", comment: "Node Exporter" }


    ## 3rd party
    node_exporter_version: '1.6.1'
    cosmos_pruner: 'https://github.com/b-harvest/cosmos-pruner'


full:
  hosts:
    s1:
      ansible_host: 1.1.1.1 # set to your own
      public_host: "{{ ansible_host }}"

```

## Roles

---

### Common Role
This role sets up the base environment for Linux Node. 

It's targeted for Ubuntu, so the other distribution may not work for this role.

Followings are included
- Installation of essential tools like `GVM`, `Node Exporter` and others ... 
- Configuration of firewall settings and network-related kernel parameters.

### Story Role
This role set up system daemon, and install other tools for operate like cosmovisor, cosmos-pruner.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes. Ensure that your code adheres to the established style and passes all tests.


