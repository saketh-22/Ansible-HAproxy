# Ansible Project: HAproxy and Flask App Deployment with UDP Loadbalancing

## Overview
This Ansible project deploys a Flask application behind HAproxy as a load balancer, with added functionalities for SNMP monitoring and Nginx UDP load balancing. The architecture consists of five hosts: HAproxy, devA, devB, devC, and Bastion.

## Architecture
- **HAproxy**: Acts as the entry point and load balancer for HTTP requests to the Flask app.
- **devA, devB, devC**: Hosts running the Flask app and SNMP daemon.
- **Bastion**: Secure SSH entry point to the internal network.

## Files
- `site.yaml`: The Ansible playbook that orchestrates the deployment of HAproxy, Nginx, and the Flask app.
- `hosts`: The inventory file that defines the hosts and their groups.
- `ssh_config`: The SSH configuration file for using the Bastion host as a jump host.
- `uipassword`: A file containing the password for accessing the HAproxy performance UI.
- `application2.py`: The Flask application that responds with the current time and hostname.

## Deployment Steps
1. **SSH Configuration**: Ensure the `ssh_config` file is set up to allow SSH access through the Bastion host.
2. **Run Playbook**: Execute the Ansible playbook using the command:

    ```bash
    ansible-playbook -i hosts site.yaml
    ```

### Flask App Deployment
- The playbook deploys the Flask app on devA, devB, and devC.
- Each server will respond to HTTP requests with the current time and hostname.

### SNMP Daemon Installation
- The playbook installs the SNMP daemon on each development server.

### Nginx Configuration
- Nginx is set up to load balance UDP traffic for SNMP (UDP port 1611 externally to UDP port 161 internally).

### HAproxy Configuration
- Load balances HTTP traffic to the Flask app.
- Performance UI is set up on port 8011, protected by a password stored in `uipassword`.

## Testing
The playbook includes a basic function test that sends three HTTP requests to the public IP of HAproxy and verifies that all three hosts (devA, devB, devC) respond correctly.

## Requirements
- Ansible installed on the local machine.
- SSH access to the Bastion host.
- SSH keys configured for password-less access to the internal hosts.

## Notes
- Ensure that the necessary ports are open on firewalls for HTTP (80), UDP (1611), and HAproxy UI (8011).

## References
- [HAProxy Configuration](https://haproxy.org/)
- [Nginx UDP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/udp-load-balancing/)
- [Flask App Repository](https://github.com/pallets/flask)
- [Ansible Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)
