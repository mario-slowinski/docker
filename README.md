docker
======

Ansible role to manager Docker objects.

Requirements
------------

* [ansible.builtin](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)

Role Variables
--------------

* defaults

  * `Debian.yaml`

    ```yaml
    Debian_pkgs: []  # list of docker packages
      - name: ""  # docker engine package
    ```

  * `RedHat.yaml`

    ```yaml
    RedHat_pkgs: []  # list of docker packages
      - name: ""  # docker engine package
    ```

  * `main.yaml`

    ```yaml
    docker_secrets: []  # list of docker secrets to manage
      - name: "plain"  # secret name
        data: !unsafe "some plain data"
      - name: "encoded"  # secret name
        data: "already base64 encoded data"
        b64: true

    docker_stacks: []  # list of docker stacks to manage
      - name: ""  # stack name
        compose:  # stack definitions in variables
          - version: '3,5'
            services: []  # list of stack services
            volumes: []  # list of stack volumes
            networks: []  # list of stack networks
            secrets: []  # list of stack secrets
      - name: ""  # stack name
        compose: "{{ lookup('file','compose.yaml') | from_yaml }}"  # stack definitions in file
    ```

Dependencies
------------

* [service](https://github.com/mario-slowinski/service)
  * [software](https://github.com/mario-slowinski/software)

Tags
----

* **docker.secret** - Manage docker secret
* **docker.stack** - Manage docker stack

Examples
--------

* `requirements.yaml`

  ```yaml
  - name: docker
    src: https://github.com/mario-slowinski/docker
  ```

* `playbook.yaml`

  ```yaml
  - hosts: servers
    gather_facts: true  # to get ansible_os_family
    roles:
      - role: docker
  ```

* manage stacks matching names with *selected stack* list

  ```sh
  ansible-playbook -t docker.stack -e docker_stack=[selected_stack] playbook.yaml
  ```

License
-------

[GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html)

Author Information
------------------

[mario.slowinski@gmail.com](mailto:mario.slowinski@gmail.com)
