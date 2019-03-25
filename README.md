# Ansible Role: Installs Apache Tomcat Java Application server (optionally with Hugepages)

Installs Apache Tomcat Java Application server. Most complete Tomcat installation, supporting, init.d script, application naming, hugepages, hardening, beautiful error pages, sha512 hashed passwords, JMX configuration, multiple Tomcat versions, separated catalina_home and caralina_base.

[![Build Status](https://travis-ci.org/KAMI911/ansible-role-tomcat-multi.svg?branch=master)](https://travis-ci.org/KAMI911/ansible-role-tomcat-multi)

## Table of Contents

1. [Requirements][Requirements]
2. [Role Variables][Role Variables]
3. [Installation][Installation]
4. [Dependencies][Dependencies]
5. [Example Playbook][Example Playbook]
6. [Licensing][Licensing]
7. [Author Information][Author Information]
8. [Support][Support]
9. [Contributing][Contributing]
10. [Donation][Donation]

## Requirements

None.

## Installation

    ansible-galaxy install kami911.tomcat-multi

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    tomcat_majorversion: 8

Tomcat major version.

    tomcat_minorversion: 5

Tomcat minor version.

    tomcat_patchversion: 4

Tomcat micro version.

    tomcat_use_huge_pages: True

Use Huge Pages (Java calls it: UseLargePages) for enhance performance of Java applications. When a process uses some memory, the CPU is marking the RAM as used by that process. For efficiency, the CPU allocate RAM by chunks of 4K bytes (it's the default value on many platforms). Those chunks are named pages. Those pages can be swapped to disk, etc.

Since the process address space are virtual, the CPU and the operating system have to remember which page belong to which process, and where it is stored. Obviously, the more pages you have, the more time it takes to find where the memory is mapped. When a process uses 1GB of memory, that's 262144 entries to look up (1GB / 4K). If one Page Table Entry consume 8bytes, that's 2MB (262144 * 8) to look-up.

[Debian Wiki: Hugepages](https://wiki.debian.org/Hugepages)

When you enable it use [KAMI911:hugepages](https://galaxy.ansible.com/KAMI911/hugepages/) to configure Huge Pages in Linux.

    tomcat_file_encoding: UTF-8

Tomcat file encoding parameter: UTF-8

    tomcat_page_encoding: UTF-8

Tomcat page encoding parameter: UTF-8

    tomcat_use_secure_flag: True

Set this attribute to True if you wish to have calls to request.isSecure() to return true for requests received by this Connector. You would want this on an SSL Connector or a non SSL connector that is receiving data from a SSL accelerator, like a crypto card, a SSL appliance or even a webserver. The default value is False.

    tomcat_session_http_only: True

Forcing Tomcat to use JSESSIONID cookie over only http.

    tomcat_session_secure: True

Forcing Tomcat to use secure JSESSIONID cookie.

    tomcat_manage_java_pkg: False

Tomcat manage java installation an install OpenJDK or not.

    tomcat_system_name: "tomcat_app"

Use this folder name for this tomcat main folder.

    tomcat_system_home: "{{ tomcat_base_folder }}/{{ tomcat_system_user }}"

Folder of Tomcat binaries using tomcat_system_home variable.

    tomcat_catalina_home: '{{ tomcat_system_home }}/tomcat{{ tomcat_majorversion }}'

Tomcat Cataline home folder.

    tomcat_instance:

List of dictioneries where the tomcat instances are configured.

      - instance_name: 'tomcat_app_sys1'

Name of first instance. Also this variable used as name of instance folder name, start/stop script and process identifier.

        http_port: 8080

Port number of http port of Tomcat instance sevice. Please define it carefuly, it should be not the same as other ports.

        http_zone: "internal"

The firewalld zone name where connection are accepted for http connections. This variable is used when firewalld supported system is used (for exmple: CentOS 7) and tomcat_manage_firewalld_use_zone variable is true.

        http_source:  # Tweak this according yout network
          - "0.0.0.0/0"

List of source ports where connection are accepted for http connections. This time only firewalld is supported. The default values are 0.0.0.0/0 means all connection is accepted. This should narrowed.

        ajb_port: 8009

Port number of ajb port of Tomcat instance sevice. Please define it carefuly, it should be not the same as other ports.

    ajb_zone: "trusted"

The firewalld zone name where connection are accepted for ajb connections. This variable is used when firewalld supported system is used (for exmple: CentOS 7) and tomcat_manage_firewalld_use_zone variable is true.

    ajb_source:  # Tweak this according yout network
      - "0.0.0.0/0"

List of source ports where connection are accepted for ajb connections. This time only firewalld is supported. The default values are 0.0.0.0/0 means all connection is accepted. This should narrowed.

    https_port: 8443

Port number of https port of Tomcat instance sevice. Please define it carefuly, it should be not the same as other ports.

    https_zone: "internal"

The firewalld zone name where connection are accepted for https connections. This variable is used when firewalld supported system is used (for exmple: CentOS 7) and tomcat_manage_firewalld_use_zone variable is true.

    https_source:  # Tweak this according yout network
      - "0.0.0.0/0"

List of source ports where connection are accepted for https connections. This time only firewalld is supported. The default values are 0.0.0.0/0 means all connection is accepted. This should narrowed.

    tomcat_manage_firewalld: true

Role manages the firewalld settings of required ports.

    tomcat_manage_firewalld_use_zone: true

Tomcat firewalld uses zones (default) or use source addresses.

## Dependencies

None.

## Example Playbook

    - hosts: all
      roles:
        - tomcat-multi

## Licensing

The lactransformer application and documantations are licensed under the terms of
the MIT / BSD, you will find a copy of this license in the
[LICENSE](LICENSE) file included in the source package.

## Author Information

This role was created in 2016-2019 by Kálmán Szalai - KAMI

## Support

If you have any question, do not hesitate and drop me a line.
If you found a bug, or have a feature request, you can [fill an issue](https://github.com/KAMI911/ansible-role-tomcat-multi/issues).

## Contributing

There are many ways to contribute to ansible-role-tomcat-multi -- whether it be sending patches,
testing, reporting bugs, or reviewing and updating the documentation. Every
contribution is appreciated!

Please continue reading in the [contributing chapter](CONTRIBUTING.md).

### Fork me on Github

https://github.com/KAMI911/ansible-role-tomcat-multi

## Donation

If you find this useful, please consider a donation:

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=RLQZ58B26XSLA)

<!-- TOC URLs -->
[Requirements]: #requirements
[Role Variables]: #role_variables
[Installation]: #installation
[Dependencies]: #dependencies
[Example Playbook]: #example_playbook
[Licensing]: #licensing
[Author Information]: #author_information
[Support]: #support
[Contributing]: #contributing
[Donation]: #donation
