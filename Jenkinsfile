@Library(‘badge-update-library’) _
inventory_cmd=‘’’cat > requirements/docker.yml << EOF
- name: Docker-Engine-Overlay
  src: git+http://github.com/pipseed/ansible-role-docker-overlay.git
  version: master
EOF’’’
playbook_cmd=‘’’cat > docker-overlay.yml << EOF
—
- hosts: “{{ chosen_hosts | default(‘none’) }}”
  become: yes
  become_method: sudo
  roles:
   - Docker-Engine-Overlay
EOF’’’
def ncDocker = null
pipeline {
    agent { label ‘ansible-master’ }
    parameters {
                string(name: ‘site’, defaultValue:  ‘http://github.com/pipseed/ansible/compute/sites/adastral-test.git’ , description: ‘IaC inventory’)
                string(name: ‘chosen_host’, defaultValue:  ‘dev-kvm-04’ , description: ‘Which host to deploy to’)
    }
    options {
      gitLabConnection(‘github.com’)
    }
 
    stages {
            stage(‘inventory’) {
                steps {
                    updateGitlabCommitStatus name: ‘build’, state: ‘pending’
                    updateBadge(“gitlab”)
                    deleteDir()
                    script{
                        def scm = git(branch: ‘master’, credentialsId: ‘gitlab’, url: params.site)
                        def workspace = pwd() //this is better approach to get working directory rather than env.workspace when running on nodes
                    }
                    configFileProvider(
                        [configFile(fileId: ‘git-credential-helper.sh’, targetLocation: ‘git-credential-helper.sh’),
                         configFile(fileId: ‘ansible_security_tokens’, targetLocation: ‘security_keys.json’)]) {
                    }
                    sh “git config —global credential.helper \”/bin/bash ${workspace}/git-credential-helper.sh\””
                }
               
            }
            stage(‘fetch roles’) {
                steps {
                        withCredentials([[
                                            $class: ‘UsernamePasswordMultiBinding’,
                                            credentialsId: ‘gitlab’,
                                            usernameVariable: ‘GIT_USERNAME’,
                                            passwordVariable: ‘GIT_PASSWORD’
                                        ]]) {
                            sh “$inventory_cmd”
                            sh “ansible-galaxy install -p roles -r requirements/docker.yml”
                            sh “git config —global —unset credential.helper”
                    }
                }
            }
            stage(‘playbook’) {
                steps {
                    sh “$playbook_cmd”
                    sh ‘ls -alrt roles/‘
                    sh ‘ansible-playbook docker-overlay.yml —extra-vars “chosen_hosts=$chosen_host, docker_user=ncauto1”’
                }
               
            }
                }
    post {
        success {
            script {
                updateGitlabCommitStatus name: ‘build’, state: ‘success’
            }
        }
        failure {
            script {
                updateGitlabCommitStatus name: ‘build’, state: ‘failed’
            }
        }
    }
}