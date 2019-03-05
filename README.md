# Datashades v2.0 Ansible playbooks and roles

This repository contains the Ansible playbooks and roles which provision infrastructure for new Datashades CKAN stacks.

### Important note

_These Ansible roles are designed to work with infrastructure provisioned using the CloudFormation stack template within the Datashades "CloudFormation" repository._

_They will fail if you attempt to run these roles against manually provisioned instances unless you ensure all of the required instance tags are in place._

## How to use

Once the CloudFormation template has created your instances, ensure your Ansible `hosts` file looks something like this:

```
[provision-services]
provision-services.hosted.zone.com ansible_ssh_private_key_file=/path/to/private/key.pem

[provision-webmaster]
provision-webmaster.hosted.zone.com ansible_ssh_private_key_file=/path/to/private/key.pem
```

1. *hosted.zone.com* should match the hosted zone you entered when using the CloudFormation template.
2. */path/to/private/key.pem* should point to the location of the SSH private key that allows access to the instances you're provisioning.

Once the `hosts` file is set up correctly, you can run the playbooks in order:

*Be sure to reboot the services instance one the run is complete, before proceeding with the webmaster playbook*

1. `ansible-playbook playbooks/services/services-ckan.yml`
2. `ansible-playbook playbooks/webmaster/webmaster-ckan.yml`

*If you wish to have Drupal installed alongside CKAN then use the `services-ckan+drupal.yml` and `webmaster-ckan+drupal.yml` playbooks instead.*

Once both playbooks have completed, you will be able to access CKAN via browser using a URL like "devweb.hosted.zone.com" (replace *dev* with *uat* or *prod* depending on your stack type.)

## Notes

1. If you provision a PROD stack, this assumes you will be using a load balanced solution. Before you will be able to access CKAN you will need to edit the load balancer and add a HTTPS listener pointing to your target group.
2. The CKAN/Drupal password will be the same as the password you entered during the CloudFormation process. The CKAN username is *sysadmin* and the Drupal username is *admin*.
