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
    triggers {
      cron '* * * * *'
      pollSCM 'H * * * *'
    }
    stages {
        stage('stage-1') {
            agent {
                label 'sonarAgent'
            }
            steps {
                git branch: 'dev', url: 'https://github.com/ChetanaMJyothi/pipeline-job-repo.git'
                echo "stage 1, Running on Sonar Agent"
                sh 'hostname'
                echo "Global Variables:-"
                echo "Branch Name: $BRANCH_NAME"
                echo "Build Number: $BUILD_NUMBER"
                echo "Job Name: $JOB_NAME"
                echo "Git URL: $GIT_URL"
            }
        }
        stage('stage-2') {
            agent {
                label 'tomcat'
            }
            steps {
                echo 'stage 2, Running on Tomcat Agent'
                sh 'hostname'
                echo "Pipeline creator name $CreatorName"
                echo "Executing in $EnvironmentName Environment"
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
                ech 'stage 3 is executing....'   
            }
            post {
                success {
                    echo 'stage 3 executed Successfully!!'   
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
                    echo "stage 4 is executing..."
                    build quietPeriod: 10, job: 'freestyle-job', waitForStart: true
                }
        }
    }
    post {
        success {
            echo "Stage 4 has executed successfully!!"
        }
        always {
            emailext body: '$DEFAULT_CONTENT', subject: '$DEFAULT_SUBJECT', to: 'chetanamj@gmail.com'
        }
    }
}
