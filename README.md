# parallelism and ansible performance (forks):

how ansible is going to handle task executions in parallel ??

by default ansible execute tasks on multipe hosts simultaneously
not one by one ..

parallelism happens because ansible creates seperate ssh connections on each managed node 

# forks :
forks are the number of ssh sessions that ansible will open to interact and perform certain tasks 
and by default it is 5 connection at a time 

# different ways of configuring forks :
- globally in config files :
etc/ansible/ansible-config -> "forks=5"

- setting environment variable:
 "export ANSIBLE_FORKS=20"

 - while running a playbook :
  "ansible-playbook playbook.yml -- forks=20"

# limiting task execution:
we can set limit for task exectution on sepcific hosts 

In a playbook using hosts:
- hosts: webservers
tasks:
- name: Restart nginx
service:
name: nginx
state: restarted

using the command :
ansible-playbook playbook.yml --limit server1,server2
eventhough we set hosts to all webservers ... we limit the servers that ansible connects to 

--limit: is a resource level : the number of resource that ansible is going to connect to 

--fork: is a task level : the number of the resources that the tasks should be executed on 


 # Serial execution:
  Serial prevents overloading everything at once during deployments.

- hosts: all
serial: 5  run only 5 servers at a time
tasks:
- name: Deploy app
shell: deploy.sh

Q: 
instead of using those flags why can't we just remove those unwanted servers from the inventory file and run ?????

A:
yes we can remove them as long as we just don't need to use them in the future 
but if we have to use them in the future reomving them and then re configure them again will take up alots of time . in this using only one or two flags while we run our commands is a way easier than removing the servers and configure then again ...


# why we should keep our inventory stable ?
- tedious to edit 
- risky 
- automation needs 

# Cloud provisioning with ansible:
ansible is agnetless and it uses cloud provider APIs in order to interact with the resources

# cloud prvisioning with ansible requires two things :
- Aws creadintails 
- Boto3 installation (pip install boto3)

# provisioning :
it is the procees of setting up resources live on the cloud without (UI)

# with ansible we can manage :
- instances
- networkings
- storage 
- DNS management


# cloud automation challenges and ansible solutions:


# challenge                                              # ansible solutions
  
- complex APIs                                           - simplified yaml playbook
- multi-cloud-needs                                      - unified interface
- state-tracking                                         - idempotent operations
- manual provisioning                                    - full automated workfolws
- security managements                                   - vaults+roles


# Monitoring with ansible 
- install agencies
- configure metrics
- automate backups 


# Orchestrating multi-tier deployments :
a multi-tier application typically has :
- frontend (ui layer)
- backend (application logic)
- database (storage)

- ansible coordinates deployments across all tier , anaging execution order , dependencies , and service configurtion to ensure that everything is setted up correctly.

# a sample deployment could look like:
- set up database 
- deploy the backend 
- configure and start services 
- deploy frontend


# CI/CD with ansible and github actions :

ci/cd with ansible and github actions is like in the same directory (repository) we add our ansible file (inventory file , palybook, roles....)



project/
│
├── .github/
│   └── workflows/
│       └── deploy.yml
│
├── inventory
├── playbook.yml
├── ansible.cfg
└── README.md
