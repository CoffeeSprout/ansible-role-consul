coffeesprout.consul
=========

The `coffeesprout.consul` Ansible role installs HashiCorp Consul, a service mesh and service discovery tool, for both client and server operations. This role uses default settings that work for the author, but allows for customization through variables.

This role supports both FreeBSD and CentOS 7/8. By default, it sets up a cluster between all the machines in the `consul-servers` group, using their private IP's to communicate. The Consul clients will join on those same IP's.

Requirements
------------

You will need to generate a Consul encryption key. Generally, the Consul binary should be available locally, or you could use Docker. Please see the [official documentation](https://www.consul.io/docs/agent/encryption.html) for more information.

Role Variables
--------------

The following variables can be customized:

- `consul_datacenter`: This variable sets the Consul datacenter. The default value is `"dc1"`.
- `download_cache`: This variable sets the directory where Consul downloads are stored. The default value is `"~/consul-downloads"`.
- `consul_dependencies`: This variable sets a list of dependencies required for Consul installation. The default value is `["unzip"]`.
- `consul_version`: This variable sets the Consul version to install. The default value is `"1.7.3"`.
- `consul_arch`: This variable sets the CPU architecture of the system. The default value is `"amd64"`.
- `consul_system`: This variable automatically selects either `linux` or `freebsd`, depending on the system.
- `consul_archive_name`: This variable sets the name of the zip file containing Consul.
- `consul_download_url`: This variable sets the URL where Consul can be downloaded.
- `consul_checksums_url`: This variable sets the URL where the checksums.txt file can be found for validating the download.
- `consul_home`: This variable sets the home folder containing the Consul configuration.
- `consul_data_dir`: This variable sets the folder containing the Consul data.
- `consul_retry_join`: This variable sets the list of IP addresses to retry joining a Consul cluster. The default value is all the server IP's in the `consul-servers` group.
- `consul_client_addr`: This variable sets the address to bind the Consul client to. The default value is `"127.0.0.1 {{ ansible_all_ipv4_addresses | ipaddr('private') | list | join(' ') }}"`, which binds to localhost and all private IP's.

Example Playbook
----------------

Here's an example playbook that uses this role:

```yaml
- hosts: consul-servers:consul-clients
  roles:
    - role: coffeesprout.consul
      consul_encryption_key: "pUqJrVyVRj5jsiYEkM/tFQYfWyJIv4s3XkvDwy7Cu5s="
        

License
-------

This role is licensed under the BSD License.

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
