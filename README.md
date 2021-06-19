# Microservices-Demo-Files
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
Pre-requesites: 
- 1 EIP for Webapp
- Change Webapp EIP in webapp deploy.yaml
- 2 ELBs
   - global-config-server-elb  - http:8888 | health_check -> /health
   - activemq-elb - http:8161 & tcp:61616 | health_check - /

Changes:

- change config-server URL in following bootstrap.properties file 
    - fleetman-webapp/src/main/resources/bootstrap.properties
    - fleetman-position-tracker/src/main/resources/bootstrap.properties
	- fleetman-position-simulator/src/main/resources/bootstrap.properties 

- Change Eureka config in the following files
   - fleetman-registry/src/main/resources/application.properties 
   - fleetman-global-config/application.properties 

- Change MQ URL in 
   - fleetman-global-config/application.properties  
   - fleetman-position-simulator/src/main/resources/application.properties	


Start-up Sequence:
- Launch Eureka - ansible-playbook launch_eureka_server_asg.yaml -i aws_ansible_inventory.yaml
- Launch global config - ansible-playbook launch_global_config_server_asg.yaml -i aws_ansible_inventory.yaml
- Launch MQ - ansible-playbook launch_activemq_asg.yaml -i aws_ansible_inventory.yaml
- build & launch position-tracker in Jenkins
- build & launch position-simulator - ansible-playbook provision_position_simulator.yaml -i aws_ansible_inventory.yaml
- build & launch webapp in Jenkins
