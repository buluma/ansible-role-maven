# [Ansible role maven](#ansible-role-maven)

Install and configure Apache Maven on your systems.

|GitHub|GitLab|Downloads|Version|
|------|------|---------|-------|
|[![github](https://github.com/buluma/ansible-role-maven/workflows/Ansible%20Molecule/badge.svg)](https://github.com/buluma/ansible-role-maven/actions)|[![gitlab](https://gitlab.com/shadowwalker/ansible-role-maven/badges/master/pipeline.svg)](https://gitlab.com/shadowwalker/ansible-role-maven)|[![downloads](https://img.shields.io/ansible/role/d/buluma/maven)](https://galaxy.ansible.com/buluma/maven)|[![Version](https://img.shields.io/github/release/buluma/ansible-role-maven.svg)](https://github.com/buluma/ansible-role-maven/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from [`molecule/default/converge.yml`](https://github.com/buluma/ansible-role-maven/blob/master/molecule/default/converge.yml) and is tested on each push, pull request and release.

```yaml
---
- become: true
  gather_facts: true
  hosts: all
  name: Converge
  pre_tasks:
  - ansible.builtin.apt:
      update_cache: true
    changed_when: false
    name: Update apt cache
    when: ansible_pkg_mgr in ('apt')
  - ansible.builtin.apt:
      name: openjdk-8-jdk
      state: present
    become: true
    name: Install jdk 8 (apt)
    when: ansible_pkg_mgr in ('apt')
  roles:
  - maven_install_dir: /opt/maven
    maven_version: 3.9.6
    role: buluma.maven
  - maven_fact_group_name: maven_3_3
    maven_is_default_installation: false
    maven_version: 3.3.9
    role: buluma.maven
```

The machine needs to be prepared. In CI this is done using [`molecule/default/prepare.yml`](https://github.com/buluma/ansible-role-maven/blob/master/molecule/default/prepare.yml):

```yaml
---
- become: true
  gather_facts: false
  hosts: all
  name: Prepare
  roles:
  - role: buluma.bootstrap
  - role: buluma.core_dependencies
  - role: buluma.buildtools
  - java_vendor: openjdk
    java_version: '11'
    role: buluma.java
  vars:
  - java_type: jdk
  - java_version: '8'
```

Also see a [full explanation and example](https://buluma.github.io/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

The default values for the variables are set in [`defaults/main.yml`](https://github.com/buluma/ansible-role-maven/blob/master/defaults/main.yml):

```yaml
---
maven_download_dir: '{{ x_ansible_download_dir | default(ansible_env.HOME + ''/.ansible/tmp/downloads'')
  }}'
maven_download_timeout: 10
maven_fact_group_name: maven
maven_install_dir: /opt/maven
maven_is_default_installation: true
maven_mirror: http://archive.apache.org/dist/maven/maven-{{ maven_version |
  regex_replace('\..*', '') }}/{{ maven_version }}/binaries
maven_use_proxy: true
maven_validate_certs: true
maven_version: 3.9.6
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

This role is part of many compatible roles. Have a look at [the documentation of these roles](https://buluma.github.io/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/buluma/ansible-role-maven/png/requirements.png "Dependencies")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/buluma):

|container|tags|
|---------|----|
|[EL](https://hub.docker.com/r/buluma/enterpriselinux)|all|
|[Fedora](https://hub.docker.com/r/buluma/fedora)|all|
|[opensuse](https://hub.docker.com/r/buluma/opensuse)|all|
|[Ubuntu](https://hub.docker.com/r/buluma/ubuntu)|all|

The minimum version of Ansible required is 2.12, tests have been done on:

- The previous version.
- The current version.
- The development version.

If you find issues, please register them on [GitHub](https://github.com/buluma/ansible-role-maven/issues).

## [License](#license)

[Apache-2.0](https://github.com/buluma/ansible-role-maven/blob/master/LICENSE).

## [Author Information](#author-information)

[buluma](https://buluma.github.io/)
