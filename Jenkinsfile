pipeline {
    agent { label ‘ansible-master’ }
    
    stages {
      stage('fetch roles') {
        steps {
          sh "ansible-galaxy install -p roles -r example/docker.yml"
        }
      }
      
      stage(‘playbook’) {
        steps {
          sh ‘ls -alrt roles/‘
          sh ‘ansible-playbook example/docker-overlay.yml —extra-vars “chosen_hosts=$chosen_host, docker_user=”seeda’
        }
      }
   }
}
