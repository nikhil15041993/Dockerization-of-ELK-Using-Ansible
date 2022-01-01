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



