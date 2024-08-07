# freebsd_bash

[![quality](https://img.shields.io/ansible/quality/27910)](https://galaxy.ansible.com/vbotka/freebsd_bash)
[![Build Status](https://travis-ci.org/vbotka/ansible-freebsd-bash.svg?branch=master)](https://travis-ci.org/vbotka/ansible-freebsd-bash)
[![GitHub tag](https://img.shields.io/github/v/tag/vbotka/ansible-freebsd-bash)](https://github.com/vbotka/ansible-freebsd-bash/tags)

[Ansible role.](https://galaxy.ansible.com/vbotka/freebsd_bash/) FreeBSD. Install /usr/local/bin/bash and change the login shell for specified users to bash.

Feel free to [share your feedback and report issues](https://github.com/vbotka/ansible-freebsd-bash/issues).

[Contributions are welcome](https://github.com/firstcontributions/first-contributions).


## tcsh vs. sh

Default login shell in FreeBSD is /bin/tcsh. This doesn't work properly with ansible as discussed in issues:

* [fatal error caused by shell type](https://github.com/ansible/ansible/issues/13459)
* [ansible_shell_type and make_become_cmd are at odds](https://github.com/ansible/ansible/issues/13179)


## Requirements

### Collections

* community.general

### Shell

It is necessary to change the login shell to /bin/sh for ansible_user
(see Edit inventory below) before applying this role.

```sh
shell> ansible host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
```


## Variables

```yaml
freebsd_bash_users	# users whose login shell will be changed
```


## Workflow

1) Install the role and collections from Ansible Galaxy https://galaxy.ansible.com/

```sh
shell> ansible-galaxy role install vbotka.freebsd_bash
shell> ansible-galaxy collection install community.general
```

2) Edit inventory

Example of a hosts file

```ini
[bsd_test]
my_host

[bsd_test:vars]
ansible_connection=ssh
ansible_user=freebsd
ansible_python_interpreter=/usr/local/bin/python3.9
ansible_perl_interpreter=/usr/local/bin/perl
```

3) Change shell (if necessary)

```sh
shell> ansible my_host -e 'ansible_shell_type=csh ansible_shell_executable=/bin/csh' -a 'sudo pw usermod freebsd -s /bin/sh'
my_host | SUCCESS | rc=0 >>
```

4) Edit playbook

```yaml
shell> cat playbook.yml
- hosts: bsd_test
  roles:
      - vbotka.freebsd_bash
```

5) Edit and change the list of users to your needs. See defaults/main.yml

6) Run playbook

```sh
shell> ansible-playbook playbook.yml
```


### Ansible lint

Use the configuration file *.ansible-lint.local* when running
*ansible-lint*. Some rules might be disabled and some warnings might
be ignored. See the notes in the configuration file.

```sh
shell> ansible-lint -c .ansible-lint.local
```


## License

[![license](https://img.shields.io/badge/license-BSD-red.svg)](https://www.freebsd.org/doc/en/articles/bsdl-gpl/article.html)


## Author Information

[Vladimir Botka](https://botka.info)
