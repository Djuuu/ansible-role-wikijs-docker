Ansible Role: Wikijs-docker
===========================

Install Wiki.js Docker Compose project.

Based on LinuxServer.io image: https://docs.linuxserver.io/images/docker-wikijs/

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common system variables:

```yaml
timezone: UTC
```

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

wikijs_project_name: wikijs

# Docker project dynamic vars (uses `docker_project_name` prefix, adapt if overridden)

wikijs_traefik_loadbalancer_server_port: 3000
wikijs_traefik_entrypoints: 'http,https'
wikijs_traefik_middlewares:
  - "https-redirect@file"

# Main service additional docker-compose options (ex: cpu_shares, deploy, ...)
wikijs_compose_service_additional_options: |
  #ports:
  #  - 3000:3000
```

```yaml
# WikiJS docker-compose vars

# Additional external docker-compose networks (ex: database)
wikijs_compose_additional_networks: []
#  - postgres_default

# Additional volumes (ex: override assets)
wikijs_compose_additional_volumes: []
#  - ./favicons:/app/wiki/assets/favicons
#  - ./favicon.ico:/app/wiki/assets/favicon.ico
#  - ./manifest.json:/app/wiki/assets/manifest.json
```

```yaml
# Wiki.js project variables

wikijs_image_version: latest

wikijs_db_type: sqlite
wikijs_db_host:
wikijs_db_port:
wikijs_db_name:
wikijs_db_user:
wikijs_db_pass:
```

Additional config files
-----------------------

Additional files in the following location will be copied in the project's directory:

- `config/wikijs/{{ inventory_hostname }}/*`

It can be useful to override assets (ex: favicons, manifest, ...).

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.wikijs_docker
```

License
-------

Beerware License
