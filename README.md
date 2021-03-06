# ![](icon.svg) Ansible Playbook for Splunk Universal Forwarder

Use this Ansible Playbook to deploy Splunk Universal Forwarder on Red Hat servers following Splunk best practices:

- The only App configured locally is the Deployment Client App
- Every other configuration is managed from the Deployment Server
- Any unmanageable configuration file is removed from `/etc/system/local`
- As it is not needed in most scenarios, admin password is randomized

**Tree view**
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
- Set Splunk UF user bash profile
- Start Splunk UF, accept license and set a random admin password
- Set OS to start Splunk UF at boot time

## Use the playbook

1. Clone repository from your Ansible controller

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

3. Add target Red Hat servers to your Ansible inventory

```
[servers]
<target servers>
```

4. Launch playbook

```
ansible-playbook -i <inventory> ansible-playbook-splunk-universal-forwarder/deploy-splunk_uf.yml -v
```

5. Push your own Apps from the Deployment Server

- An App that outputs data to your Splunk Indexer(s) (`outputs.conf`)
- Apps that handle data inputs (`inputs.conf`)
- An App that disables Splunk UF management port because it is not used
