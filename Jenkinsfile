pipeline {
    agent { label ‘ansible-master’ }
    triggers {
      githubPush()
    }
      
    
    options {
      gitLabConnection(‘github.com’)
    }
 
    stages {
            stage(‘inventory’) {
                steps {
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