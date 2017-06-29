# Ansible Role: docker-gitlab
[![Build Status](https://travis-ci.org/HP41/ansible-docker-gitlab.svg?branch=master)](https://travis-ci.org/HP41/ansible-docker-gitlab)


## Products

- **[GITLAB](https://about.gitlab.com/)**
- **[GITLAB-RUNNER](https://docs.gitlab.com/runner/)**

### Docker Images used/suggested:

- [gitlab-ce](https://hub.docker.com/r/gitlab/gitlab-ce/)
- [gitlab-runner](https://hub.docker.com/r/gitlab/gitlab-runner/)


## NOTE: The setup as a service part of this role at the moment, only support systemd. This was designed mostly for CoreOS but can easily be adapted into other systemd type distros.
- `gitlab.rb` - **If provided** process the template and places it in the appropriate location for the gitlab docker container to read it and accordingly process it.
    - For `gitlab`
    - Check out this [link](https://docs.gitlab.com/omnibus/settings/configuration.html) for info on how this can be used.
    - Also check out the template posted [here](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/files/gitlab-config-template/gitlab.rb.template).
- `config.toml` - **If provided** process the template and places it in the appropriate location for the gitlab-runner container to read and process it.
    - For `gitlab-runner`
    - Check out this [link](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/configuration/advanced-configuration.md) for info on how this can be used.
    - Also check out the template posted [here](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/config.toml.example).
- `gitlab-compose.yml` - This is the docker-compose file defining the containers to run. **If provided** process the template and places it in the appropriate location.    
    - **Please check [gitlab-compose-example.yml](gitlab-compose-example.yml) for an example on how to use it.**
    - More information regarding environment variables that can be used for the registering of `gitlab-runner` container with `gitlab` can be found [here](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner/blob/master/docs/commands/README.md#gitlab-runner-register)

## Requirements 
* Ansible >= 2.1
* Docker >= 1.9.0
* Guest machine: Debian
    - stretch
    - jessie
    - wheezy
* Guest machine: Ubuntu
    - xenial
    - trusty
    - precise

### Default Variables that can be overridden or used as-is when using this role:

- gitlab.rb
    - `docker_gitlab_gitlab_rb_template`(*Optional*): Path to the `gitlab.rb` template file in context of the role/playbook.
        - **The template processing and placing of gitlab.rb by THIS role will only happen if this variable is defined with a non-empty string**
    - `docker_gitlab_gitlab_rb_location`: Folder where `gitlab.rb` should be placed in context of the host 
        - Please ensure this location is mounted accordingly onto the container. Please check [gitlab-compose-example.yml](gitlab-compose-example.yml)
        - Default=`/opt/gitlab`
    - `docker_gitlab_gitlab_rb_file_name`: The filename for `gitlab.rb`. This is provided for flexibility 
        - Default=`gitlab.rb`
- gitlab-runner config.toml
    - `docker_gitlab_runner_config_toml_template`(*Optional*): Path to the `config.toml` template file in context of the role/playbook.
        - **The template processing and placing of config.toml by THIS role will only happen if this variable is defined with a non-empty string**
    - `docker_gitlab_runner_config_toml_location`: Folder where `config.toml` should be placed in context of the host 
        - Please ensure this location is mounted accordingly onto the container. Please check [gitlab-compose-example.yml](gitlab-compose-example.yml)
        - Default=`/opt/gitlab-runner`
    - `docker_gitlab_runner_config_toml_file_name`: The filename for `config.toml`. This is provided for flexibility 
        - Default=`config.toml`
- docker-compose
    - `docker_gitlab_docker_compose_template`(*Optional*): Path to the **docker-compose yml** template file in context of the role/playbook.
        - **The template processing and placing of docker-compose yml by THIS role will only happen if this variable is defined with a non-empty string**
    - `docker_gitlab_docker_compose_location`: Folder where **docker-compose yml** should be placed in context of the host 
        - Default=`/opt/compose-files/gitlab-docker`
    - `docker_gitlab_docker_compose_file_name`: The filename for **docker-compose yml**. 
        - Default=`gitlab-compose.yml`
- docker-compose systemd unit
    - `docker_gitlab_service_template`(*Optional*): Path to the **gitlab docker-compose systemd unit** template file in context of the role/playbook.
        - **The template processing and placing of gitlab docker-compose systemd unit by THIS role will only happen if this variable is defined with a non-empty string**
    - `docker_gitlab_service_location`: Folder where **gitlab docker-compose systemd unit** should be placed in context of the host 
        - Default=`/etc/systemd/system`
    - `docker_gitlab_service_name`: The **service name** for **gitlab docker-compose systemd unit**. 
    - **Please note, this is the service name and not the systemd service file name**
        - For eg: gitlab-docker *and NOT gitlab-docker.service* 
    - Default=`gitlab-docker`