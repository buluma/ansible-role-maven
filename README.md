# Ansible role [maven](https://galaxy.ansible.com/ui/standalone/roles/buluma/maven/documentation)

Install and configure Apache Maven on your systems.

|GitHub|Version|Issues|Pull Requests|Downloads|
|------|-------|------|-------------|---------|
|[![github](https://github.com/buluma/ansible-role-maven/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-maven/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/releases/)|[![Issues](https://img.shields.io/github/issues/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/issues/)|[![PullRequests](https://img.shields.io/github/issues-pr-closed-raw/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/pulls/)|[![Ansible Role](https://img.shields.io/ansible/role/d/buluma/maven)](https://galaxy.ansible.com/ui/standalone/roles/buluma/maven/documentation)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-maven/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
# code: language=ansible
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  pre_tasks:
    - name: Update apt cache
      ansible.builtin.apt:
        update_cache: true
      changed_when: false
      when: ansible_pkg_mgr in ('apt')

    - name: Install jdk 8 (apt)
      become: true
      ansible.builtin.apt:
        name: openjdk-8-jdk
        state: present
      when: ansible_pkg_mgr in ('apt')

  roles:
    - role: buluma.maven
      maven_version: '3.9.6'
      maven_install_dir: /opt/maven

    - role: buluma.maven
      maven_version: '3.3.9'
      maven_is_default_installation: false
      maven_fact_group_name: maven_3_3

  # post_tasks:
  #   - name: Verify default maven facts
  #     ansible.builtin.assert:
  #       that:
  #         - ansible_local.maven.general.version is defined
  #         - ansible_local.maven.general.home is defined
  #
  #   - name: Verify maven 3.3 facts
  #     ansible.builtin.assert:
  #       that:
  #         - ansible_local.maven_3_3.general.version is defined
  #         - ansible_local.maven_3_3.general.home is defined
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
      java_vendor: openjdk
      java_version: "11"
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-maven/blob/master/defaults/main.yml):

```yaml
# code: language=ansible
# https://github.com/gantsign/ansible-role-maven/blob/master/defaults/main.yml
---
# Maven version number
maven_version: '3.9.6'

# Mirror to download the Maven redistributable package from
maven_mirror: "http://archive.apache.org/dist/maven/maven-{{ maven_version | regex_replace('\\..*', '') }}/{{ maven_version }}/binaries"

# Base installation directory the Maven distribution
maven_install_dir: /opt/maven

# Directory to store files downloaded for Maven installation
maven_download_dir: "{{ x_ansible_download_dir | default(ansible_env.HOME + '/.ansible/tmp/downloads') }}"

# The number of seconds to wait before the Maven download times-out
maven_download_timeout: 10

# Whether to use the proxy when downloading Maven (if the proxy environment variable is present)
maven_use_proxy: true

# Whether to validate HTTPS certificates when downloading Maven
maven_validate_certs: true

# If this is the default installation, symbolic links to mvn and mvnDebug will
# be created in /usr/local/bin
maven_is_default_installation: true

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

| Requirement | GitHub | Version |
|-------------|--------|--------|
|[buluma.bootstrap](https://galaxy.ansible.com/buluma/bootstrap)|[![Ansible Molecule](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-bootstrap/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-bootstrap.svg)](https://github.com/shadowwalker/ansible-role-bootstrap)|
|[buluma.buildtools](https://galaxy.ansible.com/buluma/buildtools)|[![Ansible Molecule](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-buildtools/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-buildtools.svg)](https://github.com/shadowwalker/ansible-role-buildtools)|
|[buluma.core_dependencies](https://galaxy.ansible.com/buluma/core_dependencies)|[![Ansible Molecule](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-core_dependencies/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-core_dependencies.svg)](https://github.com/shadowwalker/ansible-role-core_dependencies)|
|[buluma.java](https://galaxy.ansible.com/buluma/java)|[![Ansible Molecule](https://github.com/buluma/ansible-role-java/actions/workflows/molecule.yml/badge.svg)](https://github.com/buluma/ansible-role-java/actions/workflows/molecule.yml)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-java.svg)](https://github.com/shadowwalker/ansible-role-java)|

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:

![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-maven/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/repository/docker/buluma/enterpriselinux/general)|8, 9|
|[Fedora](https://hub.docker.com/repository/docker/buluma/fedora/general)|39, 38|
|[opensuse](https://hub.docker.com/repository/docker/buluma/opensuse/general)|all|
|[Ubuntu](https://hub.docker.com/repository/docker/buluma/ubuntu/general)|bionic, focal, jammy|

The minimum version of Ansible required is 2.12, tests have been done to:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them in [GitHub](https://github.com/buluma/ansible-role-maven/issues)

## [Changelog](#changelog)

[Role History](https://github.com/buluma/ansible-role-maven/blob/master/CHANGELOG.md)

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-maven/blob/master/LICENSE)

## [Author Information](#author-information)

[Shadow Walker](https://buluma.github.io/)

