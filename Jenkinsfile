def stage2Status='SUCCESS'
def stage3Status='FAIL'
pipeline {
    agent any
    stages {
        stage('stage-1') {
            steps {
                echo "stage 1"
                git branch: 'dev', url: 'https://github.com/ChetanaMJyothi/pipeline-job-repo.git'
            }
        }
        stage('stage-2') {
            steps {
                echo 'Hello stage 2'
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
}
