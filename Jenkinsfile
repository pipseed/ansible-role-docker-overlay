pipeline {
    agent { label 'ansible-master' }
    parameters {
      string(name: 'chosen_host', defaultValue:  'dev-kvm-10' ,description: 'Application Repository name from git')
      string(name: 'site', defaultValue:  'http://github.ams1.info/ansible/compute/sites/jenkins.git' ,description: 'My-Test Site')
    }
 
    stages {
      stage('fetch roles') {
        steps {
          sh "ansible-galaxy install -p roles -r provision/docker-overlay.yml"
        }
      }
      
      stage('playbook') {
        steps {
          ansiblePlaybook colorized: true, 
          installation: 'ansible',
          inventory: 'provision/hosts', 
          playbook: 'provision/docker.yml',
          extras: 'chosen_hosts=$chosen_hosts, docker_user=seeda'
        }
      }
   }
}
