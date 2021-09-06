# mongodb-ansible

To execute the playbook:

ansible-playbook mongo-playbook.yml -vvvv -i ./mongo-hosts 

Here, -vvv is used to run the ansible playbook in verbose mode. If we dont mention the hosts file(using i flag) , ansible-playbook will pick from the default one. In case of Ubuntu, that is /var/ansible/hosts.
