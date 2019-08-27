Ansible Docker Deployment Role
==============================

Ansible role for deploying a docker container on a host and use this container as an Ansible host.

Requirements
------------

The role requires Python 3.5+ to be installed on the host.

Role Variables
--------------

| Variable | Default | Description |
|----------|---------|-------------|
| docker_registry | https://index.docker.io|Registry containing the docker image. |
| docker_registry_user | **required** | Username for the registry account. |
| docker_registry_passwd | **required** | Password for the registry account. |
| docker_registry_certificate | | Registry certificate. |
| docker_image | **required** | Image name or ID. |
| docker_container | **required** | Name assigned to the container. |
| docker_container_user | root:root | User running the container. |
| docker_shared_mounts | [] | Volumes to mount within the container. For the syntax, see the [docker_container](https://docs.ansible.com/ansible/latest/modules/docker_container_module.html) documentation. |
| docker_published_ports | [] | Ports to publish from the container to the host. or the syntax, see the [docker_container](https://docs.ansible.com/ansible/latest/modules/docker_container_module.html) documentation. |
| docker_install_remote_api | true | Whether to install the Docker Remote API that allows connection to the Docker container via TCP. |
| docker_remote_api_port | 4243 | Port used to connect to the remote docker host with the Docker Engine API. |

Example Playbook
----------------

Here is an example of how to use the role:

    - hosts: servers

      pre_tasks:
        - name: '[DOCKER] Set ansible python interpreter to Python 3'
          set_fact:
            ansible_python_interpreter: /usr/bin/python3
          tags:
            - always

      roles:
         - role: jcpassy.ansible-docker-deployment-role
           docker_registry_user: toto
           docker_registry_passwd: titi
           docker_image: my_image:latest
           docker_docker_container: my_image_container

If you install the Docker Remote API, you will be able to treat the container as an Ansible host and run Ansible scripts on it.
For this purpose, the Docker container should be defined the following way in your inventory file:

    my_image_container ansible_connection=docker ansible_docker_extra_args="-H tcp://<host_machine>:<docker_remote_api_port>"

where `<host_machine>` is the host machine of the docker and `<docker_remote_api_port>` is the port defined above.

License
-------

BSD 3-Clause "New" or "Revised" License

Author Information
------------------

[Jean-Claude Passy](https://github.com/jcpassy)

Comments on the Ansible, PR or bug reports are welcome.