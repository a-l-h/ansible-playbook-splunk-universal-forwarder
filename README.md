# ![](logo.svg) Ansible Playbook for Splunk Universal Forwarder

Use this Ansible Playbook to deploy Splunk Universal Forwarder on Linux servers

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

#### User/Group

- Add group
- Add user

#### Install / Upgrade Splunk Universal Forwarder

- Stop Splunk Universal Forwarder
- Unpack Splunk Universal Forwarder tgz file
- Unpack Deployment Client base App to `/etc/apps`
- **rename deployment client app**
- Add the Deployment Server URI to `deploymentclient.conf`
- Remove any unneeded configuration file from `/etc/system/local`
- Transfer `/apps/splunkcx2/splunkforwarder` ownership to `<splunk_user>` user
- Set Splunk user bash profile
- Start Splunk, accept license and set a random admin password

## Use the playbook

1. Clone repository from your Ansible Controller server

```
git clone ...
```
2. Adjust variables as needed ./main.yml

| variable                      | example value                                       |
|-                              |-                                                    |
| splunk_uf_tgz                 | splunkforwarder-8.0.6-152fb4b2bb96-Linux-x86_64.tgz |
| splunk_ds_fqdn                | mycomp.ds                                           |
| splunk_ds_port                | 8089                                                |
| splunk_install_dir            | /opt                                                |
| splunk_user                   | splunk                                              |
| splunk_user_group             | splunk                                              |
| privilege_escalation          | sudo                                                |

company acronym

2. Add latest splunk forwarder to ./roles/common/files

```
wget ...
```
3. Add target Linux servers in your Ansible inventory

4. Launch playbook with 

```
ansible-playbook
```

Explanations

Best practice to only have deploymentclient conf from the universal forwarder
Then form the DS, push output / inputs App
Push an app to disable mgmt port
Explain random admin pwd
nothing in system/local -> cannot be managed
Explain bash propfile ?

usage all vars as extra var
stop if not defined
