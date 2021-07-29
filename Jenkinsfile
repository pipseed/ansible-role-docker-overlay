pipeline {
    agent { label 'ansible-master' }
    parameters {
      string(name: 'chosen_host', defaultValue:  'dev-kvm-10' ,description: 'Application Repository name from git')
    }
 
    stages {
      stage('fetch roles') {
        steps {
          sh "ansible-galaxy install -p roles -r example/docker-overlay.yml"
        }
      }
      
      stage(‘playbook’) {
        steps {
          sh "ansible-playbook example/docker.yml —extra-vars 'chosen_hosts=$chosen_host, docker_user=seeda"
        }
      }
   }
}
