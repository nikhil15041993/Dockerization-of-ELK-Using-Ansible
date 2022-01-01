# Dockerization-of-ELK-Using-Ansible


Running this playbook will perform the following actions on your Ansible hosts:

- [x]  739
- [ ] https://github.com/octo-org/octo-repo/issues/740
- [ ] Add delight to the experience when all tasks are complete :tada:


Install aptitude, which is preferred by Ansible as an alternative to the apt package manager.
Install the required system packages.
Install the Docker GPG APT key.
Add the official Docker repository to the apt sources.
Install Docker.
Install the Python Docker module via pip.
Pull the default image specified by default_container_image from Docker Hub.
Create the number of containers defined by the create_containers variable, each using the image defined by default_container_image, and execute the command defined in default_container_command in each new container.
