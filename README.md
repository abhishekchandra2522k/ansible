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

2. $ `ansible webservers -m yum -a "name=httpd state=absent" -i inventory --become`

- General - $`ansible <host/group name> -m <module name> "<options>" -i <inventory file path> --become`
- We use `--become` to execute the command on the target machine with sudo privileges.

3. To execute a playbook -> $ `ansible-playbook -i inventory web-db.yml`

4. To execute a playbook in the debug mode $ `ansible-playbook -i inventory web-db.yml -v` (add one more v for 2nd level of verbose mode)

5. To execute a playbook in the debug mode with much more info (3rd level verbose) $ `ansible-playbook -i inventory web-db.yml -vvv`, we can add one more v (4th level - final verbose).

6. To check the playbook syntax - $ `ansible-playbook -i inventory web-db.yml --syntax-check`

7. To dry run the playbook - $ `ansible-playbook -i inventory web-db.yml -C` - It's a good practice to run a dry run before the final execution of the playbook.

8. To pass variables via cmd line - $ `ansible-playbook -e USRNM=cliuser -e COMM=cliuser vars_precedence.yaml` - the vars passed in the cmd line have the highest priority

### Some Important Ansible informational points

1. Add `gather_facts: False` to disable the [Gathering Facts] tasks where the fact variables are initialized.

2. $ `ansible -m setup web01` - to see the fact variables value for web01 machine.

3. Copy Module vs Template module in Ansible

- copy module takes the file and directly dumps in the target location.
- template module is intelligent, it will read the file, if we have any Jinja2 template, what's Jinja2 template? the structure that we use, variables "{{<var_name>}}", conditions `when:`, loops `loop:` this is Jinja2 templating. Template module is going to look for any templating that we may have done, and then from that extract the actual content and push it to the target location. basically it will fetch the values of the vars...

4. $ `sudo apt install tree -y` - install tree module to see the dir structure in a tree format - $ `tree <dir-path>`

5. $ `ansible-galaxy init post-install` -- To create a new role (module as in terraform) - "post-install" will be created...

6. Use Ansible Galaxy documentation to use public built roles for various tasks - for example installing java on the system, we can use $ `ansible-galaxy install geerlingguy.java`, this will help download the java role contents into our directory

7. vim editor tip: `:%s/^<space><space><space>//` replace 3 spaces with nothing -- to remove the unnecessary indentation.