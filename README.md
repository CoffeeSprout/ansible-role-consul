coffeesprout.consul
=========

Installs Consul both for client and server operations. There are other roles out there that do much more, but none of them were simple enough.
This role uses defaults that work for me and requires just the encryption key.

Supports both FreeBSD and CentOS 7/8
By default it will setup a cluster between all the machines in the group consul-servers, using their private IP's to communicate. The Consul clients will join on those same IP's

Requirements
------------

You will need to generate a Consul encryption key. Generally I have the Consul binary locally or you could use docker, have a look at the [official documentation](https://www.consul.io/docs/agent/encryption.html)
I use the key from their example in my example, please generate your own :-)
The defaults also require netaddr to be installed on the Ansible controller; Either override them or install the pip module

Role Variables
--------------

    consul_datacenter: "dc1"

Consul datacenter; see https://www.consul.io/docs/agent/options.html#_datacenter

    download_cache: "~/consul-downloads"

Where to store the consul downloads

    consul_dependencies:
    - unzip

We need to be able to unarchive, which requires the zip binary

    consul_version: "1.7.3"

Generally the latest stable version we can find

    consul_arch: "amd64"

Default to the AMD64 arch

    consul_system: "{{ ansible_system | lower }}"

Will automatically select linux or freebsd

    consul_archive_name: "consul_{{ consul_version }}_{{ consul_system }}_{{ consul_arch}}.zip"

The name of the zip file containing Consul

    consul_download_url: "https://releases.hashicorp.com/consul/{{ consul_version }}/{{ consul_archive_name }}"

Where to download Consul

    consul_checksums_url: "sha256:https://releases.hashicorp.com/consul/{{ consul_version }}/consul_{{ consul_version }}_SHA256SUMS"

Where to find the checksums.txt to validate the download

    consul_home: /usr/local/etc/consul.d

The home folder containing the Consul configuration

    consul_data_dir: /opt/consul

The folder containing the Consul data

    consul_retry_join: "{{ groups['consul-servers']|map('extract', hostvars, ['ansible_default_ipv4','address'] )| list }}"

By default we populate the retry_join list with all the server IP's. We assume you are using an internal network to provision Ansible, but feel free to change this variable. Just offer it as a list of IP's

    consul_client_addr: "127.0.0.1 {{ ansible_all_ipv4_addresses | ipaddr('private') | list | join(' ') }}"

Where to bind the client_addr, default to localhost and all private IP's


Example Playbook
----------------

As always with my role, there are a few required settings that you need to specify. Below is the minimum to get this rolling

    - hosts: consul-servers:consul-clients
      roles:
      - role: coffeesprout.consul
        consul_encryption_key: "pUqJrVyVRj5jsiYEkM/tFQYfWyJIv4s3XkvDwy7Cu5s="
        

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
