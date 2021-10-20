pipeline {
    agent { label 'ansible-master' }
    environment {
          PATH="/home/auto-test/.local/bin:${env.PATH}"
    }
    parameters {
      string(name: 'chosen_hosts', defaultValue:  'dev-kvm-10' ,description: 'Application Repository name from git')
      string(name: 'site', defaultValue:  'http://github.ams1.info/ansible/compute/sites/jenkins.git' ,description: 'My-Test Site')
    }
 
    stages {
      stage('Fetch Roles') {
        steps {
          sh "ansible-galaxy install -p provision/roles -r provision/docker-overlay.yml"
        }
      }
      
      stage('Run Playbook') {
        steps {
          sh "ansible-playbook provision/docker.yml -i provision/hosts -e 'chosen_hosts=$chosen_hosts, docker_user=seeda'"
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
