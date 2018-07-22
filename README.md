freebsd-bash
============

[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-bash.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-bash)
[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd-bash/) FreeBSD. Install /usr/local/bin/bash and change the login shell for specified users to bash.


tcsh vs sh
----------

Default login shell in FreeBSD is /bin/tcsh. This doesn't work properly with ansible as discussed in issues:

- [fatal error caused by shell type](https://github.com/ansible/ansible/issues/13459)

- [ansible_shell_type and make_become_cmd are at odds](https://github.com/ansible/ansible/issues/13179)


Requirements
------------

It is necessary to change the login shell to /bin/sh for ansible_user (see Edit inventory below) before applying this role .

```
ansible host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'

```


Variables
---------

```
freebsd_bash_users	# users whos' login shell will be changed
```


Workflow
--------

1) Install the role from Ansible Galaxy https://galaxy.ansible.com/.

```
ansible-galaxy install vbotka.freebsd-bash
```

2) Edit inventory.

Example of a hosts file.

```
[bsd-test]
<ip-or-hostname>

[bsd-test:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python2.7
ansible_perl_interpreter=/usr/local/bin/perl
```

3) Change shell (if necessary).

```
ansible host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
<ip_sanitized> | SUCCESS | rc=0 >>
```

4) Edit playbook.

```
cat freebsd-bash.yml

- hosts: bsd-test
  roles:
      - vbotka.freebsd-bash
```

5) Edit list of users. For example in vars/main.yml.

6) Run playbook.

```
ansible-playbook freebsd-bash.yml
```


License
-------

BSD


Author Information
------------------

[Vladimir Botka](https://botka.link)
