# Snowplow Development Environment

[Vagrant] [vagrant]-based development environment with [Ansible] [ansible] playbooks to install common tools, including:

* The JVM ecosystem
* Ruby
* Postgres

Works fine on Linux, Mac and Windows hosts.

Used as the development environment for all [Snowplow Analytics] [snowplow] projects.

## Installation

### Dependencies

To use this development environment, you need to have [Vagrant] [vagrant-install] and [VirtualBox] [virtualbox-install] installed.

We also recommend installing vagrant-vbguest to prevent the VirtualBox Guest Additions from getting out of sync:

	$ vagrant plugin install vagrant-vbguest

### Starting Vagrant

First, clone the repo:

	$ git clone git@github.com:snowplow/dev-environment.git
	$ cd dev-environment

Now you can build the VM:

	$ vagrant up

And SSH into it:

	$ vagrant ssh

### Installing software

The guest VM has Ansible installed. This means you can run the different [Ansible playbooks] [ansible-pb] directly, thus:

```
$ ansible-playbook /vagrant/ansible-playbooks/{{PLAYBOOK_NAME}}.yaml \
--inventory-file=/vagrant/home/ansible/ansible_hosts --connection=local
```

For example, to run the 'base' playbook:

```
$ ansible-playbook /vagrant/ansible-playbooks/generic/base.yaml \
--inventory-file=/home/vagrant/ansible_hosts --connection=local
```

To install the JVM 6 environment:

```
$ ansible-playbook /vagrant/ansible-playbooks/generic/jvm/jvm-6.yaml \
--inventory-file=/home/vagrant/ansible_hosts --connection=local
```

### Starting developing

We recommend removing git tracking from the dev environment before starting coding. You can do these either from the host or the guest VM:

    $ rm -rf .git*

Now you can safely pull down the codebase you want to work on e.g:

    $ git clone git@github.com:snowplow/snowplow.git

## Available playbooks

### Generics

The [`/ansible-playbooks/generic`] [generic-pb] folder contains the available playbooks:

| Category   | Name                                | Description                                                                      | Dependencies |
|:-----------|:------------------------------------|:---------------------------------------------------------------------------------|:-------------|
| -          | [`base.yaml`] [base-pb]             | Installs basic utilities that are useful on the dev box e.g. Git, Vim etc.       | None         |
| `jvm`      | [`jvm-6.yaml`] [jvm6-pb]            | <ul><li>Oracle Java 1.6 (1.6 not v1.7, for Amazon EMR compat)</li><li>Scala 2.10.3</li><li>SBT 0.13.0</li><li>Thrift 0.9.1</li></ul> | None         |
| `jvm`      | [`play-2.yaml`] [play-2-pb]         | Installs the Play 2 Framework                                                    | `jvm-6.yaml` |
| `db`       | [`postgres-8.4.yaml`] [postgres-8.4-pb] | Installs Postgres 8.4. (8.4 not 9, for Amazon Redshift compat)         | None         |
| `ruby`     | [`ruby-rvm.yaml`][ruby-rvm-pb]      | Installs RVM, Ruby version to 1.9.3 and sets default Ruby to 1.9.3               | None         |

### Vendors

The [`/ansible-playbooks/vendor`] [vendor-pb] folder contains the available playbooks:

| Vendor                  | Name                                                  | Description                                                  | Dependencies |
|:------------------------|:------------------------------------------------------|:-------------------------------------------------------------|:-------------|
| `com.snowplowanalytics` | [`snowplow.github.com.yaml`] [snowplow.github.com-pb] | Ruby, Jekyll & Pygments for the Snowplow website's front-end | `ruby`       |

## Copyright and license

Snowplow Development Environment is copyright 2014 Snowplow Analytics Ltd.

Licensed under the [Apache License, Version 2.0] [license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[vagrant]: http://vagrantup.com
[vagrant-install]: http://docs.vagrantup.com/v2/installation/index.html
[virtualbox]: https://www.virtualbox.org
[virtualbox-install]: https://www.virtualbox.org/wiki/Downloads
[ansible]: http://www.ansibleworks.com/

[snowplow]: http://snowplowanalytics.com

[ansible-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks
[generic-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/generic
[vendor-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/vendor

[base-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/generic/base.yaml
[jvm6-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/generic/jvm/jvm-6.yaml
[play-2-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/generic/jvm/play-2.yaml
[postgres-8.4-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/generic/db/postgres-8.4.yaml
[ruby-rvm-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/generic/ruby/ruby-rvm.yaml

[snowplow.github.com-pb]: /snowplow/dev-environment/blob/master/ansible-playbooks/vendor/com.snowplowanalytics/snowplow.github.com.yaml

[license]: http://www.apache.org/licenses/LICENSE-2.0
