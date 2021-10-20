pipeline {
    agent { label 'ansible-master' }
    parameters {
      string(name: 'chosen_hosts', defaultValue:  'dev-kvm-10' ,description: 'Application Repository name from git')
      string(name: 'site', defaultValue:  'http://github.ams1.info/ansible/compute/sites/jenkins.git' ,description: 'My-Test Site')
    }
 
    stages {
      stage('Fetch Roles') {
        steps {
          sh "id"
          sh "/home/auto-test/.local/bin/ansible-galaxy install -p provision/roles -r provision/docker-overlay.yml"
        }
      }
      
      stage('Run Playbook') {
        steps {
          sh "pwd"
					sh "tree"
          ansiblePlaybook colorized: true, 
          extras: '-e "chosen_hosts=$chosen_hosts, docker_user=seeda"',
          installation: '/home/auto-test/.local/bin/ansible',
          inventory: 'provision/hosts', 
          playbook: 'provision/docker.yml'
        }
      }
      stage('Validate') {
        steps {
          sh "/home/auto-test/.local/bin/molecule test"
        }
      }
   }
 
}
