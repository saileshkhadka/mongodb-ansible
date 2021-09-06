# mongodb-ansible
Task-04 MongoDB Setup and configuration through Ansible
In the assessment 04 I have to do some setup and configuration of MongoDB as per the requirements given.
Pre-requisites:
-	We have to install Ansible on the system first.
Then, writing an ansible file to configure mongodb using playbook.yml i.e.

Mongo-playbook.yml

hosts: 10.0.0.11
  become: true
  serial: 1

  tasks:
    - name: Install aptitude using apt
      apt:
        name: aptitude
        state: latest
        update_cache: yes

    - name: Import public key
      apt_key:
        url: 'https://www.mongodb.org/static/pgp/server-4.2.asc'
        state: present

    - name: Add repository
      apt_repository:
        filename: '/etc/apt/sources.list.d/mongodb-org-4.2.list'
        repo: 'deb https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse'
        state: present
        update_cache: yes

    - name: Install mongoDB
      apt:
        name: mongodb-org
        state: present
        update_cache: yes

    - name: Ensure mongodb is running and and enabled to start automatically on reboots
      service:
        name: mongod
        enabled: yes
        state: started

In my scenario, I have created mongo-hosts file so that when running ansible-playbook command, the host file which has the all the required host information where it consists of name of all the servers on which we need mongodb installed.
i.e. 

mongo-hosts

[local]
localhost       ansible_connection=local

[mongo-server-1]
10.0.0.11
[mongo-servers:children]
mongo-server-1

So here we can see 
-	Hosts represents the instances on which playbook will run.
-	tasks are the steps that ansible will take on each server.

To execute the playbook:
ansible-playbook mongo-playbook.yml -vvvv -i ./mongo-hosts 

Here, -vvv is used to run the ansible playbook in verbose mode. If we dont mention the hosts file(using i flag) , ansible-playbook will pick from the default one. In case of Ubuntu, that is /var/ansible/hosts.

After this a mongodb application is setup on node machine which runs on background and also restart if there is any process failure.
 

Privileges and Access

Creating database:

I have created two different database with some user privileges and access i.e, 
 
Here mongo_task01 and mongo_task02 are two different databases.

For mongo_task01 database, 

User task01 have read only permission 
 
For mongo_task02 database, 

User task02 have both read/write permission, 
 

Next, I have created admin user with all privileges for all database i.e.,
 
With having root privileges it can access all dbs.
Finally, Inserting some data using MongoDB Cli on the database I created i.e.,

db.createCollection("Employee01")
db.Employee01.insert({"emp_name":"aslesh",
          "city":"lalitpur",
          "address":"manbhawan",
          "hobbies":["playing",
                "reading",
                "swimming"]})
                
                
db.createCollection("Student01")
db.Student01.insert({"name":"aslesh",
          "city":"lalitpur",
          "address":"manbhawan",
          "course":["python",
                "django",
                "devops"]})

