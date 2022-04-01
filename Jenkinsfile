pipeline {
    agent any
    post {
        success {
            updateGitlabCommitStatus name: 'checkout', state: 'success'
            updateGitlabCommitStatus name: 'package', state: 'success'
            updateGitlabCommitStatus name: 'deploy', state: 'success'
        }
        failure {
            updateGitlabCommitStatus name: 'checkout', state: 'failed'
            updateGitlabCommitStatus name: 'package', state: 'failed'
            updateGitlabCommitStatus name: 'deploy', state: 'failed'
        }
    }
    tools {
        maven 'maven_3.8.5'
        jdk 'jdk8'
    }
    options {
        gitLabConnection('gitlab_local')
        gitlabBuilds(builds: ['checkout', 'test', 'package', 'deploy'])
    }
    stages {
        stage('checkout') {
            steps {
                echo 'checkout from branch: ' + env.gitlabBranch
                checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: env.gitlabBranch]], extensions: [], userRemoteConfigs: [[credentialsId: 'Rex_Gitlab', url: 'http://host.docker.internal:8887/rex/hello-world.git']]]
				echo 'checkout end'
            }
        }
        stage('package') {
            steps {
                echo 'package start'
				sh 'mvn clean package'
                echo 'package end'
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy start'
				// sh 'mvn clean package'
                echo 'deploy end'
            }
        }
    }
}