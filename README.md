# ![](logo.svg) Ansible Playbook for Splunk Universal Forwarder

Use this Ansible Playbook to deploy Splunk Universal Forwarder on Linux servers following Splunk best practices:

- The only App configured locally on the forwarder is the Deployment Client App
- Every other configuration is managed from the Deployment Server
- No unmanageable configuration file is allowed in /etc/system/local

**Content**
```
📦 ansible-playbook-splunk-universal-forwarder
 ┣ 📂 roles
 ┃ ┗ 📂 controller
 ┃ ┃ ┗ 📂 defaults
 ┃ ┃ ┃ ┗ 📜 main.yml
 ┃ ┃ ┗ 📂 tasks
 ┃ ┃   ┗ 📜 main.yml
 ┃ ┗ 📂 forwarders
 ┃   ┗ 📂 defaults
 ┃   ┃ ┗ 📜 main.yml
 ┃   ┗ 📂 tasks
 ┃     ┗ 📜 main.yml
 ┣ 📜 deploy-splunk_uf.yml
 ┗ 📜 README.md
 ```

## Playbook main steps

#### On Ansible controller

- Download Splunk UF latest version
- Check MD5 hash

#### On target servers

- Proceed if target is a 64-bit server
- Proceed if target is a Red Hat server

##### User/Group

- Add splunk group
- Add splunk user

##### Install / Upgrade Splunk UF

- Stop Splunk UF if needed
- Unpack Splunk UF TGZ file
- Create Deployment Client base App
- Remove any unneeded configuration file from `/etc/system/local`
- Transfer `/opt/splunkforwarder` ownership to splunk user
- Set Splunk user bash profile
- Start Splunk, accept license and set a random admin password
- Set OS to start Splunk UF at boot time

## Use the playbook

1. Clone repository from your Ansible controller server

```
git clone https://github.com/a-l-h/ansible-playbook-splunk-universal-forwarder.git
```

2. Adjust variables as needed from each role's `defaults/main.yml` file

#### controller

| variable                 | default value             |
|-                         |-                          |
| controller_become_method | sudo                      |

#### forwarders

| variable                 | default value             |
|-                         |-                          |
| splunk_uf_install_dir    | /opt                      |
| splunk_uf_user           | splunk                    |
| splunk_uf_user_group     | splunk                    |
| splunk_uf_become_method  | sudo                      |
| company_acronym          | org                       |
| splunk_ds_fqdn           | org.deploymentserver.fqdn |
| splunk_ds_port           | 8089                      |

3. Add target Linux servers in your Ansible inventory

```
[servers]
<target servers>
```

4. Launch playbook

```
ansible-playbook -i <inventory> ansible-playbook-splunk-universal-forwarder/deploy-splunk_uf.yml -v
```
