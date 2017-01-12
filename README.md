# Snowplow Ansible Playbooks

[Ansible] [ansible] playbooks to install common tools, including:

* The JVM ecosystem
* Ruby
* Postgres

Currently these are all used to configure Vagrant VMs (specifically [dev-environment] (https://github.com/snowplow/dev-environment)) to hack on specific parts of the Snowplow stack. Going forwards, we plan to extend the playbooks to also manage server environments at [Snowplow Analytics] [snowplow].

We have tried to write each playbook in a generic way, so that this may be a useful resource if you want to set up similar development environments that are not related to Snowplow specifically.

### Repo structure

Plays to install individual bundles of software e.g. Ruby / RVM, Java, Scala, SBT, Thrift, NodeJS etc. have each been bundled into separate [roles] (https://github.com/snowplow/ansible-playbooks/tree/master/roles).

Those roles are then combined into larger playbooks that are saved in the project root directory. For example, the [snowplow-batch-pipeline.yml] (https://github.com/snowplow/ansible-playbooks/blob/master/snowplow-batch-pipeline.yml) runs the base, Java, Scala, SBT and Ruby / RVM roles required to enable development on the Snowplow Hadoop-based data pipeline.

This structure makes it simple to compose new playbooks out of the roles. For example, to create a playbook that installs Ruby / RVM and PostgreSQL, we'd create a new playbook file e.g. `ruby-postgres.yml` with the following contents:

```
---
- hosts: vagrant
  remote_user: vagrant
  roles:
    - base
    - ruby-rvm
    - postgres
```

## Running the playbooks to install packages on local machine (e.g. Vagrant VM)

Once you've SSHed into Vagrant,

```
ansible-playbook boto.yml -i /vagrant/home/ansible/ansible_hosts --connection=local
```

Note you can update the reference to the hosts file (`/vagrant/home/ansible/ansible_hosts`) to point at any file on the VM with the following contents:

```
[vagrant]
127.0.0.1:2222
```

## Installing specific versions of different packages e.g. Postgres

We are moving to making it possible to decide, at run time, what version of each package you wish to install. This has been implemented with the Postgres playbook, for example.

By default, running the Postgres playbook will install version 8.4. (That default is specified in `ansible-playbooks/roles/postgres/defaults/main.yml`).

If instead you'd like to install Postgres 9.3, simply execute:

```
ansible-playbook /vagrant/ansible-playbooks/postgres.yml -i /vagrant/home/ansible/ansible_hosts --connection=local --extra-vars "postgres_version=9.3"
```

For more information on roles in Ansible playbooks, consult the [Ansible documentation] [ansible-roles-documentation]

## Running the playbooks against a remote server (e.g. on EC2)

To use the different roles in this repo to install the applications on a remote server e.g. on EC2:

1. Update your hosts / inventory file with details of the server you wish to install the apps on
2. Create a new playbook pointing at the appropriate host in your inventory file, and set to use the appropriate user
3. Run it! Note that in general, we add the `---ask-sudo-pass` flag, 

For example, to install any of the applications to a box called 'snowplow-external-server-1', make sure that the `~/.ssh/config` file contains something like the following:

```
...
Host snowplow-external-server-1
  Hostname ec2-11-222-333-4444.compute-1.amazonaws.com
  User username_with_sudo_permissions
  IdentityFile "~/.ssh/id_rsa"
  Port 22
  ForwardAgent yes
 ...

```

Then Update the hosts file:

```
[snowplow-external-server-1]
snowplow-external-server-1
```

Then create a playbook like the following:

```yaml
- hosts: snowplow-external-server-1
  remote_user: username_with_sudo_permissions
  
  roles:
   - role: oracle-java 
```

## A note on RStudio setup

RStudio uses PAM to do user management. Once you've run the RStudio Server playbook, create a new user to log in with:

```
$ sudo adduser rstudio
```

Assign that user a password. On your host machine, you'll then be able to log in (e.g. to localhost:8788) with the credentials you just created on the Ubuntu guest.

## Copyright and license

Snowplow Ansible Playbooks is copyright 2014 - 2017 Snowplow Analytics Ltd.

Licensed under the [Apache License, Version 2.0] [license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[ansible]: https://www.ansible.com/
[ansible-roles-documentation]: http://docs.ansible.com/playbooks_roles.html
[snowplow]: http://snowplowanalytics.com


[license]: http://www.apache.org/licenses/LICENSE-2.0
