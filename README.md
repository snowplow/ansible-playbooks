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


## Installing specific versions of different packages e.g. Postgres

We are moving to making it possible to decide, at run time, what version of each package you wish to install. This has been implemented with the Postgres playbook, for example.

By default, running the Postgres playbook will install version 8.4. (That default is specified in `ansible-playbooks/roles/postgres/defaults/main.yml`).

If instead you'd like to install Postgres 9.3, simply execute:

```
ansible-playbook /vagrant/ansible-playbooks/postgres.yml -i /vagrant/home/ansible/ansible_hosts --connection=local --extra-vars "postgres_version=9.3"
```

For more information on roles in Ansible playbooks, consult the [Ansible documentation] [ansible-roles-documentation]


## Copyright and license

Snowplow Ansible Playbooks is copyright 2014 Snowplow Analytics Ltd.

Licensed under the [Apache License, Version 2.0] [license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[ansible]: http://www.ansibleworks.com/
[ansible-roles-documentation]: http://docs.ansible.com/playbooks_roles.html
[snowplow]: http://snowplowanalytics.com


[license]: http://www.apache.org/licenses/LICENSE-2.0
