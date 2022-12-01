# Dockerization-of-ELK-Using-Ansible


Running this playbook will perform the following actions on your Ansible hosts:

:heavy_check_mark:  Install Ansible

:heavy_check_mark: Install aptitude, which is preferred by Ansible as an alternative to the apt package manager.

:heavy_check_mark:  Install the required system packages.

:heavy_check_mark:  Install the Docker GPG APT key.

:heavy_check_mark:  Add the official Docker repository to the apt sources.

:heavy_check_mark:  Install Docker.

:heavy_check_mark:  Install the Python Docker module via pip.

:heavy_check_mark:  Pull the default image specified by default_container_image from Docker Hub.

:heavy_check_mark:  Create the number of containers defined by the create_containers variable, each using the image defined by default_container_image, and execute the command defined in default_container_command in each new container.



## Install Ansible 

```

sudo apt-get update

sudo apt-get install software-properties-common

sudo apt-add-repository --yes --update ppa:ansible/ansible

sudo apt-get install ansible

# Test if it's working

ansible --version

# First Command

ansible all -i localhost, --connection=local -m ping
```

## Playbook.yml

 

```

---
- name: Docker setup
  hosts: remote
  become: true

  tasks:
     - name: Install prerequisites
       apt: name=aptitude update_cache=yes state=latest force_apt_get=yes

     - name: instal requir packages.
       apt: name={{ item }} update_cache=yes state=latest
       loop: [ 'apt-transport-https', 'ca-certificates', 'curl', 'software-properties-common', 'python3-pip', 'virtualenv', 'python3-setuptools']

     - name: add gpt key
       apt_key:
          url: https://download.docker.com/linux/ubuntu/gpg
          state: present

     - name: add docker repository to apt
       apt_repository:
          repo: deb https://download.docker.com/linux/ubuntu bionic stable
          state: present

     - name: install docker
       apt: name={{ item }} update_cache=yes state=latest
       loop: [ docker-ce, docker-ce-cli, containerd.io ]

     - name: start docker
       service:
          name: docker
          state: started
          enabled: yes

     - name: Install Docker Module for Python
       pip:
          name: docker


     - name:  vm.max_map_count kernel setting must be set to at least 262144
       shell:
         "sysctl -w vm.max_map_count=300000"


     - name: pull elasticsearch
       docker_container:
          name: elasticsearch
          image: docker.elastic.co/elasticsearch/elasticsearch:7.9.2
          env:
             node.name="es01"
             cluster.name="cluster"
             discovery.seed_hosts="es01"
             cluster.initial_master_nodes="es01"
             bootstrap.memory_lock="true"
             xpack.security.enabled="true"
             ELASTIC_PASSWORD="password"
             "ES_JAVA_OPTS=-Xms512m -Xmx512m"

          ulimits:
          - 'memlock:-1:-1'

          ports:
             - 9200:9200  


     - name: Copy kibana.yml file
       template: 
          src: "/home/nikhils/Documents/Automation/ELK/kiban.yml.j2" 
          dest: "/home/ubuntu/"


     - name: pull kibana
       docker_container:
          name: kibana
          image: kibana:7.9.2
          volumes:
          - /home/ubuntu/kiban.yml.j2:/usr/share/kibana/config/kibana.yml

          ports:
             - 5601:5601     


     - name: copy logstash configuration file
       template:
          src: "/home/nikhils/Documents/Automation/ELK/beats.conf.j2"
          dest: "/home/ubuntu/"
         


     - name: pull logstash
       docker_container: 
          name: logstash
          image: logstash:7.9.2
          volumes:
             - type: bind
               source: /home/ubuntu/logstash_pipeline
               target: /usr/share/logstash/pipeline/
               
         

```



