# 4640-w10-lab-start-w25

4640-w10-lab-start-w25
Infrastructure lab: build AMI with Packer + Ansible, deploy EC2 via Terraform module, then destroy.

Structure
Terraform root: main.tf, provider.tf
Module: main.tf, variables.tf, outputs.tf
Packer: ansible-web.pkr.hcl, variables..hcl, playbook.yml
Key scripts: import_lab_key, delete_lab_key
Module
Resource aws_instance.web with variables: project_name, ami, instance_type (default t2.micro), key_name, vpc_security_group_ids, subnet_id. Outputs: instance_ip, instance_dns, instance_id (see outputs.tf).

Build AMI
sh packer init packer/ansible-web.pkr.hcl packer build -var-file=packer/variables.pkr.hcl packer/ansible-web.pkr.hcl

Record ami id from output
Import SSH key
sh ./scripts/import_lab_key

Record key pair name
Terraform deploy
sh cd terraform terraform init terraform apply -var "ami=ami-xxxxxxxx" -var "key_name=YOUR_KEY_NAME"

Outputs: module.web_server.instance_ip / instance_dns / instance_id
Test SSH
sh ssh -i key_data/lab_key ubuntu@<public_ip>

Clean up
sh terraform destroy -var "ami=ami-xxxxxxxx" -var "key_name=YOUR_KEY_NAME" ./scripts/delete_lab_key

Deregister AMI & delete snapshot (console or AWS CLI)
Notes
Update security group and subnet references in root main.tf.
Keep provider settings in provider.tf.
