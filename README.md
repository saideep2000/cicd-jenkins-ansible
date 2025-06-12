# CI/CD Pipeline with Jenkins and Ansible

A complete CI/CD pipeline demonstration using Docker Compose, Jenkins, and Ansible to automate web application deployment.

## Architecture

- **Jenkins**: CI/CD pipeline orchestration
- **Ansible**: Configuration management and deployment
- **Web Servers**: Two Ubuntu containers running Nginx
- **Application**: Simple HTML/CSS web app

## Project Structure

```
cicd-jenkins-ansible/
├── docker-compose.yml          # Container orchestration
├── Jenkinsfile                 # CI/CD pipeline definition
├── .gitignore                  # Git ignore rules
├── app/                        # Web application
│   ├── index.html
│   └── styles.css
├── ansible/                    # Ansible configuration
│   ├── ansible.cfg
│   ├── inventory/
│   │   └── hosts.yml
│   ├── playbooks/
│   │   ├── configure-webservers.yml
│   │   └── deploy-app.yml
│   └── templates/
│       ├── index.html.j2
│       └── nginx.conf.j2
├── jenkins/                    # Custom Jenkins image
│   └── Dockerfile
└── jenkins_home/              # Jenkins data (ignored by Git)
```

## Quick Start

### 1. Start the Infrastructure

```bash
# Start all containers
docker compose up -d

# Verify containers are running
docker compose ps
```

### 2. Setup Jenkins

1. Access Jenkins: http://localhost:8080
2. Get initial password:
   ```bash
   docker compose logs jenkins | grep -A 2 -B 2 "initialAdminPassword"
   ```
3. Complete Jenkins setup (install suggested plugins)
4. Create pipeline job named `web-app-pipeline`

### 3. Test Ansible (Optional)

```bash
# Enter Ansible control container
docker compose exec ansible-control sh

# Install sshpass
apk add sshpass

# Test connectivity
ansible webservers -m ping

# Run initial server configuration
ansible-playbook playbooks/configure-webservers.yml
```

### 4. Run CI/CD Pipeline

1. Go to Jenkins → web-app-pipeline
2. Click "Build Now"
3. Monitor pipeline execution

## Access Points

- **Jenkins Dashboard**: http://localhost:8080
- **Web App (Server 1)**: http://localhost:8081
- **Web App (Server 2)**: http://localhost:8082

## Pipeline Stages

1. **Checkout**: Code verification
2. **Build**: Application preparation
3. **Test**: Unit test execution
4. **SonarQube Analysis**: Code quality check (placeholder)
5. **Deploy**: Ansible-based deployment to web servers

## Manual Commands

### Container Management

```bash
# Stop all containers
docker compose down

# Restart specific service
docker compose restart jenkins

# Rebuild with changes
docker compose up -d --build

# View logs
docker compose logs jenkins
```

### Ansible Operations

```bash
# Test server connectivity
docker compose exec jenkins bash -c "cd /ansible && ansible webservers -m ping"

# Run deployment manually
docker compose exec jenkins bash -c "cd /ansible && ansible-playbook playbooks/deploy-app.yml"

# Check Nginx status
docker compose exec jenkins bash -c "cd /ansible && ansible webservers -m shell -a 'systemctl status nginx'"
```

## Troubleshooting

### Common Issues

1. **Jenkins can't connect to web servers**:
   - Check if all containers are in the same network
   - Verify SSH services are running on web servers

2. **Pipeline fails at Deploy stage**:
   - Ensure Ansible files are mounted correctly
   - Check SSH connectivity from Jenkins to web servers

3. **Web servers not responding**:
   - Verify Nginx is installed and running
   - Check port mappings in docker-compose.yml

### Debugging Commands

```bash
# Check container status
docker compose ps

# Check container logs
docker compose logs [service_name]

# Enter container for debugging
docker compose exec [service_name] bash

# Test network connectivity
docker compose exec jenkins ping web1
```

## Development Workflow

1. **Make changes** to application code in `app/` directory
2. **Run pipeline** in Jenkins to deploy changes
3. **Verify deployment** at http://localhost:8081 and http://localhost:8082
4. **Monitor logs** for any issues

## Next Steps

- [ ] Connect to real Git repository
- [ ] Add SonarQube container for code analysis
- [ ] Implement proper testing framework
- [ ] Add database deployment
- [ ] Set up monitoring and alerting
- [ ] Add production environment

## Technologies Used

- **Docker & Docker Compose**: Containerization
- **Jenkins**: CI/CD automation
- **Ansible**: Configuration management
- **Nginx**: Web server
- **Ubuntu**: Base OS for web servers

---

**Note**: This is a development setup. For production use, implement proper security measures, use environment variables for sensitive data, and configure proper networking.