# [Ansible role maven](#maven)

Install and configure Apache Maven on your systems.

|GitHub|GitLab|Downloads|Version|Issues|Pull Requests|
|------|------|-------|-------|------|-------------|
|[![github](https://github.com/buluma/ansible-role-maven/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-maven/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-maven/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-maven)|[![downloads](https://img.shields.io/ansible/role/d/4762)](https://galaxy.ansible.com/buluma/maven)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/pulls/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-maven/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- name: Converge
  hosts: all

  pre_tasks:
    - name: Update apt cache.
      ansible.builtin.apt: update_cache=true cache_valid_time=600
      when: ansible_os_family == 'Debian'

    - name: update apt cache
      ansible.builtin.apt:
        update_cache: yes
      when: ansible_pkg_mgr == "apt"

  roles:
    - role: buluma.maven
      maven_version: '3.8.5'
      maven_install_dir: /opt/maven

    - role: buluma.maven
      maven_version: '3.3.9'
      maven_is_default_installation: no
      maven_fact_group_name: maven_3_3

  post_tasks:
    - name: verify default maven facts
      ansible.builtin.assert:
        that:
          - ansible_local.maven.general.version is defined
          - ansible_local.maven.general.home is defined

    - name: verify maven 3.3 facts
      ansible.builtin.assert:
        that:
          - ansible_local.maven_3_3.general.version is defined
          - ansible_local.maven_3_3.general.home is defined
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-maven/blob/master/molecule/default/prepare.yml):

```yaml
---
- name: Prepare
  hosts: all
  gather_facts: no
  become: yes
  vars:
    - java_type: jdk
    - java_version: "8"

  roles:
    - role: buluma.bootstrap
    - role: buluma.core_dependencies
    - role: buluma.buildtools
    - role: buluma.java
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-maven/blob/master/defaults/main.yml):

```yaml
---
# Maven version number
maven_version: '3.8.5'

# Mirror to download the Maven redistributable package from
maven_mirror: "http://archive.apache.org/dist/maven/maven-{{ maven_version|regex_replace('\\..*', '') }}/{{ maven_version }}/binaries"

# Base installation directory the Maven distribution
maven_install_dir: /opt/maven

# Directory to store files downloaded for Maven installation
maven_download_dir: "{{ x_ansible_download_dir | default(ansible_env.HOME + '/.ansible/tmp/downloads') }}"

# The number of seconds to wait before the Maven download times-out
maven_download_timeout: 10

# Whether to use the proxy when downloading Maven (if the proxy environment variable is present)
maven_use_proxy: yes

# Whether to validate HTTPS certificates when downloading Maven
maven_validate_certs: yes

# If this is the default installation, symbolic links to mvn and mvnDebug will
# be created in /usr/local/bin
maven_is_default_installation: yes

# Name of the group of Ansible facts relating this Maven installation.
#
# Override if you want use this role more than once to install multiple versions
# of Maven.
#
# e.g. maven_fact_group_name: maven_3_3
# would change the Maven home fact to:
# ansible_local.maven_3_2.general.home
maven_fact_group_name: maven
```

## [Requirements](#requirements)

- pip packages listed in [requirements.txt](https://github.com/buluma/ansible-role-maven/blob/master/requirements.txt).

## [State of used roles](#state-of-used-roles)

The following roles are used to prepare a system. You can prepare your system in another way.

| Requirement | GitHub | GitLab |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Build Status GitHub](https://github.com/buluma/ansible-role-bootstrap/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-bootstrap/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Build Status GitHub](https://github.com/buluma/ansible-role-buildtools/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-buildtools/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-buildtools)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Build Status GitHub](https://github.com/buluma/ansible-role-core_dependencies/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-core_dependencies/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.java](https://galaxy.ansible.com/buluma/java)|[![Build Status GitHub](https://github.com/buluma/ansible-role-java/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-java/actions)|[![Build Status GitLab](https://gitlab.com/shadowwalker/ansible-role-java/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-java)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-maven/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|8|
|[Fedora](https://hub.docker.com/repository/docker/buluma/fedora/general)|34, 35, 36|
|[OpenSUSE](https://hub.docker.com/repository/docker/buluma/opensuse/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|all|
|[Debian](https://hub.docker.com/repository/docker/buluma/debian/general)|all|

The minimum version of Ansible required is 2.1, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-maven/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-maven/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-maven/blob/master/LICENSE).

## [Author Information](#author-information)

[Michael Buluma](https://buluma.github.io/)

Please consider [sponsoring me](https://github.com/sponsors/buluma).

### [Special Thanks](#special-thanks)

Template inspired by [Robert de Bock](https://github.com/robertdebock)
