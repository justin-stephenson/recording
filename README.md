recording
=========

This role configures a system for [Terminal session recording](https://github.com/scribery).

Requirements
------------

This role has no required pre-requisites currently.

Role Variables
--------------

Configure session recording with SSSD, the preferred way of managing recorded users or groups:

- `recording_use_sssd` (default: `True`)

Configure SSSD recording scope - `all` / `some` / `none`:

- `recording_scope_sssd` (default: `none`)

Comma-separated list of users to be recorded ( e.g. recordeduser, testuser1 ):

- `recording_users_sssd` (default: `""`)

Comma-separated list of groups to be recorded ( e.g. recordedgroup, wheel, ):

- `recording_groups_sssd` (default: `""`)

Install`the cockpit-session-recording`package:

- `recording_install_session_player` (default: `False`)

Log writer type(output destination) of tlog-rec-session. Possible values are: `rsyslog` , `journal`:

- `recording_output` (default: `journal`)

Elasticsearch hostname, used when session recording is configured to send to Elasticsearch through rsyslog:

- `recording_elastic_host` (default: `localhost`)

Restart the Cockpit service:

- `recording_restart_cockpit` (default: `False`)

Enable Cockpit to start at system boot (this will not start Cockpit right away, only after reboot)

- `recording_enable_cockpit` (default: `False`)



Dependencies
------------

This role has no dependencies currently.

Example Playbook
----------------
~~~
---
- name: SR
  become: yes
  hosts: all
  roles:
    - role: recording
      vars:
          recording_scope_sssd: "some"
          recording_users_sssd: "recordeduser"
          recording_install_session_player: yes
          recording_restart_cockpit: yes
          recording_enable_cockpit: yes
~~~
Testing
-------
In-tree tests are provided that use molecule to test the role against docker containers.
These tests are designed to be used by CI, but they can also be run locally to test it
out while developing.  This is best done by installing molecule in a virtualenv:

  `$ virtualenv .venv`

  `$ source .venv/bin/activate`

  `$ pip install molecule docker selinux`

It is required to run the tests as a user who is authorized to run the 'docker' command
without using sudo.  This is typically accomplished by adding your user to the 'docker'
group on your system.

Once your virtualenv is properly set up, the tests can be run with these commands:

  `$ molecule test`

By default, the test target will be the latest `centos` image from Docker Hub.  You
can test against a different image/tag like so:

  `$ MOLECULE_DISTRO="fedora:28" molecule test`

License
-------

GPL v3.0

Author Information
------------------

- Nathan Kinder @nkinder

- Kirill Glebov @sabbaka
