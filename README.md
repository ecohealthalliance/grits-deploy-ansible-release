### GRITS installation

This project provides installation scripts for the GRITS suite, including the 
[diagnostic-dashboard](https://github.com/ecohealthalliance/diagnostic-dashboard-release) 
user interface, the [grits-api](https://github.com/ecohealthalliance/grits-api-release) 
backend, the [girder](https://github.com/girder/girder) database, and 
all dependencies.


**Note: This repository contains git submodules that you must initialize before
deployment.**
```
git submodule init && git submodule update
```

### Configuration

Before using this repository, use [ansible-vault](http://docs.ansible.com/playbooks_vault.html) to create a secure.yml file with sensitive passwords and other information. Copy the format in secure.yml.sample.

Also, change the domains in group_vars/dev and group_vars/prod to the domains you will deploy to, and
edit the inventory.ini file to add instance ip addresses you want to deploy to.

### Deploying to an AWS instance

We recommend using a c3.xlarge ubuntu instance. 

Once you have gone through all the configuration steps run a command like this:

```
ansible-playbook site.yml -i inventory.ini --vault-password-file ~/.grits_vault_password --private-key ~/.keys/grits-dev.pem
```

Add `--extra-vars "reindex=true"` to regenerate the elasticsearch index
if it gets messed up.

For production deployments use prod-playbook.yml instead of site.yml.

On the deployment target most of the grits components are
installed as user `grits`.  So to inspect the install use `sudo su - grits`. 
Cron logs go in `/home/grits/cron/logs` by default and supervisor logs are in
`/var/log/supervisor/`. Elasticsearch logs are in `/var/log/elasticsearch/elasticsearch.log`.

Ansible can also provision new instances, however we haven't used this feature yet.

Helpful links:
 * http://docs.ansible.com/guide_aws.html
 * http://docs.ansible.com/intro_dynamic_inventory.html#example-aws-ec2-external-inventory-script
