@Library('pipeline_library@lib') _
properties([gitLabConnection(gitLabConnection: '', jobCredentialId: ''), parameters([string('Url'), string('branch')])])

pipeline {
    agent any
    stages {
        stage('Git'){
            steps {
                script{
                //config = readProperties file: 'Configuration'
                //echo "$config.git_url"
                //echo "$config.git_branch"
                kumar.clonegit()
                }
            }
        }
        stage('Code stability'){
            steps {
                script{
                kumar.jobmvn('compile')
                }
            }
        }
        stage('Code Quality'){
            steps {
                script{
                   kumar.jobmvn('checkstyle:checkstyle')
                }
            }
        }
         stage('Code Coverage'){
            steps {
                script{
                   kumar.jobmvn('cobertura:cobertura')
                }
            }   
        }
        stage('Code Coverage Report'){
            steps {
                script{
                cobertura coberturaReportFile: '**target/site/cobertura/coverage.xml'
                }
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
        failure {
            slackSend (color: '#00FF00', message: "Job failure: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'")
            mail to: 'shitunjay.kumar@mygurukulam.org',
            subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
            body: "Something is wrong with ${env.BUILD_URL}"
        }
        success {
            slackSend (color: '#FF0000', message: "Job Success: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'")
            mail to: 'shitunjay.kumar@mygurukulam.org',
            subject: "Pipeline successful: ${currentBuild.fullDisplayName}",
            body: "This build is successful with ${env.BUILD_URL}"
        }
    }
}
