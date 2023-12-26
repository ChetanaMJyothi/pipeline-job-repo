def stage2Status='SUCCESS'
def stage3Status='FAIL'
pipeline {
    agent none
    parameters {
        credentials credentialType: 'org.jenkinsci.plugins.plaincredentials.impl.StringCredentialsImpl', defaultValue: 'sonarToken', name: 'SecretToken', required: false
        password defaultValue: 'Renuka$96', name: 'Password'
        string defaultValue: 'Chetana', description: 'MyName', name: 'CreatorName'
        choice choices: ['Development', 'UAT', 'Production'], name: 'EnvironmentName'
        booleanParam defaultValue: true, name: 'Execute'
    }
    stages {
        stage('stage-1') {
            agent {
                label 'sonarAgent'
            }
            steps {
                echo "stage 1"
                git branch: 'dev', url: 'https://github.com/ChetanaMJyothi/pipeline-job-repo.git'
            }
        }
        stage('stage-2') {
            agent {
                label 'tomcat'
            }
            steps {
                echo 'Hello stage 2'
                echo "Pipeline creator name $CreatorName"
                echo "Executing in $EnvironmentName"
            }
            post {
                success {
                    script {
                        stage2Status='SUCCESS'
                    }
                }
            }
        }
        stage('stage-3') {
            when {
                anyOf {
                    branch 'dev'
                    branch 'master'
                }
                expression { stage2Status == 'SUCCESS' }
            }
            steps {
                echo 'Hello stage 3'
            }
            post {
                success {
                    script {
                        stage3Status='SUCCESS'
                    }
                }
            }
        }
        stage('stage-4') {
            when {
                expression { stage3Status == 'SUCCESS' }
            }
                steps {
                    echo "stage 4"
                    build quietPeriod: 15, job: 'freestyle-job', waitForStart: true
                }
        }
    }
    post {
        always {
            emailext body: '$DEFAULT_CONTENT', subject: '$DEFAULT_SUBJECT', to: 'chetanamj@gmail.com'
        }
    }
}