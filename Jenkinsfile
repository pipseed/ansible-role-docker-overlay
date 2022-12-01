pipeline {
    agent { label 'aws-sandpit' }

    environment {
          MS_TEAMS              = credentials("O365_URL")
          PATH                  = "/home/ubuntu/.local/bin:${env.PATH}"
          penv                  = params.env.trim().toLowerCase()
          TFPATH                = "/usr/local/bin"
          AWS_ACCESS_KEY_ID     = credentials('aws-pipseed-dev')
          AWS_SECRET_ACCESS_KEY = credentials('aws-pipseed-dev')
          AWS_DEFAULT_REGION    = "eu-west-1"
    }
    parameters {
    choice(
      name: 'Site',
      choices: ['AWS', 'Home', 'Test'],
      description: 'Site Location:\nHome\nAWS\nTesting'
    )
    choice(
      name: 'Host',
      choices: ['dev-kvm-10', 'dev-kvm-09', 'dev-kvm-08', 'dev-kvm-07'],
      description: 'Host to deploy to......'
    )
    }

    stages {
      stage("Get Roles") {
        steps {
          script {
            sh """
            echo "Installing Ansible Galaxy Roles"
            source /home/ubuntu/jenkins-workspace/jenkins-venv/bin/activate
            ansible-galaxy install -p provision/roles -r provision/docker-overlay.yml
            """
          }
        }
      }
      
      stage('Run Playbook') {
        steps {
          script {
            sh """
            echo "Installing Docker Overlay"
            ansible-playbook provision/docker.yml -i provision/hosts -e 'chosen_hosts=${params.Host}, docker_user=seeda'
            """
          }
        }
      }
      stage('Molecule Validation') {
        steps {
          script {
            sh """
            molecule test
            """
          }
        }
      }
    }
   post {
     always {
        deleteDir()
        dir("${env.WORKSPACE}@tmp") {
            deleteDir()
        }
     }
   } 
}