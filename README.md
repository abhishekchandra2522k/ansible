# ansible

### Inventory

Inventory file in Ansible basically provides information to ansible about its targets, its IP addresses, username, password, login keys, port numbers, whatever it needs to login into the target machine.

Two ways of writing inventory file - INI format, YAML (new added)

Default inventory file - /etc/ansbile/hosts

### ad hoc commands in Ansible

1. How to not make the ansible login interactive to the target machine, as it asks the permission to store the hosts fingerprint to remember it, if you login the next time it will not ask, but we need it at the first attempt to establish the connection as it is going to be automated. We are going to use the below approach:

- $ `sudo cat /etc/ansible/ansible.cfg` (ansible.cfg is the ansible configuration file)

- Create a backup of the above config file (renaming the current file)- $ cd /etc/ansible -> $ `mv ansible.cfg ansible_backup.cfg`

- Create a new config file - $ `ansible-config init --disabled -t all > ansible.cfg`

- $ `vim ansible.cfg` (we need to disable the host_key_checking)

- Find host_key_checking variable and mark it to False, make sure its uncommented as well (host_key_checking=False).

- $ `chmod 400 clientkey.pem` (making the public key open)

  - 400: This is an octal representation of the desired permissions. The three digits represent permissions for:
    - Owner (User): The first digit (4) grants read permission to the file's owner.
    - Group: The second digit (0) grants no permissions to the group that owns the file.
    - Others: The third digit (0) grants no permissions to any other users on the system.
  - Breakdown of the octal value 4:
    - 4: Read (r)
    - 2: Write (w)
    - 1: Execute (x)

- $ `ansible web01 -m ping -i inventory` (This is an adhoc command to run ping module on the web01 host taken from the inventory file)


2. $ `ansible webservers