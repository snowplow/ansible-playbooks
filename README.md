# Snowplow Ansible Playbooks

[Ansible] [ansible] playbooks to install common tools, including:

* The JVM ecosystem
* Ruby
* Postgres

Used for configuring development and server environments.

### Generic playbooks

The [`/playbooks/generic`] [generic-pb] folder contains the available playbooks:

| Category   | Name                                | Description                                                                      | Dependencies |
|:-----------|:------------------------------------|:---------------------------------------------------------------------------------|:-------------|
| -          | [`base.yaml`] [base-pb]             | Installs basic utilities that are useful on the dev box e.g. Git, Vim etc.       | None         |
| `jvm`      | [`jvm-6.yaml`] [jvm6-pb]            | <ul><li>Oracle Java 1.6 (1.6 not v1.7, for Amazon EMR compat)</li><li>Scala 2.10.3</li><li>SBT 0.13.0</li><li>Thrift 0.9.1</li></ul> | None         |
| `jvm`      | [`play-2.yaml`] [play-2-pb]         | Installs the Play 2 Framework                                                    | `jvm-6.yaml` |
| `db`       | [`postgres-8.4.yaml`] [postgres-8.4-pb] | Installs Postgres 8.4. (8.4 not 9, for Amazon Redshift compat)         | None         |
| `ruby`     | [`ruby-rvm.yaml`][ruby-rvm-pb]      | Installs RVM, Ruby version to 1.9.3 and sets default Ruby to 1.9.3               | None         |

### Vendor playbooks

The [`/playbooks/vendor`] [vendor-pb] folder contains the available playbooks:

| Vendor                  | Name                                                  | Description                                                  | Dependencies |
|:------------------------|:------------------------------------------------------|:-------------------------------------------------------------|:-------------|
| `com.snowplowanalytics` | [`snowplow.github.com.yaml`] [snowplow.github.com-pb] | Ruby, Jekyll & Pygments for the Snowplow website's front-end | `ruby`       |

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

[snowplow]: http://snowplowanalytics.com


[generic-pb]: /snowplow/dev-environment/blob/master/playbooks/generic
[vendor-pb]: /snowplow/dev-environment/blob/master/playbooks/vendor

[base-pb]: /snowplow/dev-environment/blob/master/playbooks/generic/base.yaml
[jvm6-pb]: /snowplow/dev-environment/blob/master/playbooks/generic/jvm/jvm-6.yaml
[play-2-pb]: /snowplow/dev-environment/blob/master/playbooks/generic/jvm/play-2.yaml
[postgres-8.4-pb]: /snowplow/dev-environment/blob/master/playbooks/generic/db/postgres-8.4.yaml
[ruby-rvm-pb]: /snowplow/dev-environment/blob/master/playbooks/generic/ruby/ruby-rvm.yaml

[snowplow.github.com-pb]: /snowplow/dev-environment/blob/master/playbooks/vendor/com.snowplowanalytics/snowplow.github.com.yaml

[license]: http://www.apache.org/licenses/LICENSE-2.0
