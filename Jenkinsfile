pipeline {
    agent { label 'ansible-master' }
    parameters {
    choice(
      name: 'Host',
      choices: ['dev-kvm-10', 'dev-kvm-09', 'dev-kvm-08', 'dev-kvm-07'],
      description: 'Application Repository name from git'
    )
    choice(
      name: 'Site',
      choices: ['dev', 'test', 'prod'],
      description: 'Environment'
    )
    }
    environment {
          PATH="/home/auto-test/.local/bin:${env.PATH}"
    }
 
    stages {
      stage('Fetch Roles') {
        steps {
          sh "ansible-galaxy install -p provision/roles -r provision/docker-overlay.yml"
        }
      }
      
      stage('Run Playbook') {
        steps {
          sh "ansible-playbook provision/docker.yml -i provision/hosts -e 'chosen_hosts=$Host, docker_user=seeda'"
        }
      }
      stage('Molecule Validation') {
        steps {
          sh "molecule test"
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
