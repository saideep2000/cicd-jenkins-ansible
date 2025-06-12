pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                echo 'Code checked out from Git'
                // Git checkout happens automatically
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building application...'
                sh 'ls -la /app/'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Running unit tests on HTML/CSS"'
                // Add your test commands here
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                sh 'echo "SonarQube analysis completed"'
                // We'll add real SonarQube later
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying with Ansible...'
                sh '''
                cd /ansible
                ansible-playbook playbooks/deploy-app.yml
                '''
            }
        }
    }
}

// 7258fbdb734e46eb8cc4ac969d610ce4