<img style="float:left" alight="left" height="128px" width="100px" src="https://github.com/fubarhouse/ansible-role-golang/raw/master/gopher.png">

# Ansible Role: Go

[![Build Status](https://img.shields.io/travis/fubarhouse/ansible-role-golang/master.svg?style=for-the-badge)](https://travis-ci.org/fubarhouse/ansible-role-golang)
[![stability-stable](https://img.shields.io/badge/stability-stable-green.svg?style=for-the-badge)](https://github.com/orangemug/stability-badges)
[![Ansible Galaxy](https://img.shields.io/ansible/role/14309.svg?style=for-the-badge)](https://galaxy.ansible.com/fubarhouse/golang)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge)](https://raw.githubusercontent.com/fubarhouse/ansible-role-golang/master/LICENSE)

* Installs the Google [Go](https://golang.org/) programming language
* Install configurations are entirely automatic
* Install configurations can be manually set
* Installs from configurable mirror
* Optionally cleans your `$GOPATH` whenever you need to.
* Installs optional packages using `go get` and/or `go install`.

## Requirements

  None.

## Role Variables

Installation configuration
````yaml
go_custom_mirror: https://storage.googleapis.com/golang
````

Basic configuration
````yaml
go_version: 1.10beta2
GOPATH: /home/vagrant/go
GOROOT: /usr/local/go
````

Optional configuration
````yaml
GOOS: darwin
GOARCH: amd64
go_checksum: sha256:82628a1a42d7ad88b100d0c4c9c0282a7e008e4eb73876bed4bd61ac4ee11b46
````

****Building from source****

Golang Bootstrap Workspace
````yaml
GOROOT_BOOTSTRAP: /home/vagrant/go1.4
````
Boolean to indicate build should be from source.
````yaml
build_go_from_source: false
````
Boolean to indicate Bootstrap needs installation.
````yaml
install_go_bootstrap: false
````
Which script should be used when building from source
````yaml
go_build_script: make.bash
````

To install `go get` binaries/projects, add them to `go_get`
````yaml
go_get:
- name: gopm
  url: github.com/gpmgo/gopm
- name: golint
  url: github.com/golang/lint/golint
````

You can also manually clone and get specific versions of packages, which does not include the download of any dependencies.

This was due to the need to install specific versions of software written in Go, and the language provides no alternative at this time.

It's highly recommend you run this playbook as many times as you need until you can get a success and to use `go_reget` in conjunction with this feature, or to not use this feature unless absolutely necessary. 
````yaml
go_install:
  # repo is the git clone url, ssh or https.
- repo: https://github.com/fubarhouse/dvm.git
  # dest is the namespace
  dest: github.com/fubarhouse/dvm
  # version refers to a tag, or branch.
  version: 2.2.5
````

To ensure all packages are removed before running the play, you can use the go_reget variable:
````yaml
go_reget: true
````

To add/change the absolute path of shell profiles to configure, use `golang_shell_profile`.

**If you do not define `golang_shell_profile`, the functionality will be ignored.**

````yaml
golang_shell_profile: /root/.bash_profile
````

To clean up an installation completely prior to role execution:
````yaml
go_install_clean: true
````
### Setting permissions
**Note**: If you specify insufficient permissions the playbook will treat the following play as a new installation because it will not be able to determine what version is installed. 
To specify the permissions of the codebase, you can set:
````yaml
mode_codebase: 0755
````
To specify the permissions of the workspace, you can set:
````yaml
mode_workspace: 0755
````

## Dependencies

None.

## Example Playbook

````
- hosts: localhost
  roles:
    - fubarhouse.golang
````

## Installation

* Install using `ansible-galaxy install fubarhouse.golang`
* Add this role to your playbook.
* Modify above variables as desired.

## License

MIT / BSD

## Author Information

This role was created in 2016 by [Karl Hepworth](https://twitter.com/fubarhouse).

Image of Go's mascot was created by [Takuya Ueda](https://twitter.com/tenntenn). Licenced under the Creative Commons 3.0 Attributions license. This image has been resized for purpose, but is otherwise unchanged.