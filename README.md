# README.md

# Start with the new configuration
docker compose up -d

# Stop any existing containers
docker compose down

docker compose ps

# Enter the Ansible control container
docker compose exec ansible-control sh

# To install sshpass in the ansible container
apk add sshpass

# test connectivity
ansible webservers -m ping

# Run the playbook to install and configure Nginux
ansible-playbook playbooks/configure-webservers.yml

# Test the websites:
From inside the container: curl http://web1 and curl http://web2
From your browser: http://localhost:8081 and http://localhost:8082