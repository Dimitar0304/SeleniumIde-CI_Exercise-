pipeline {
    agent any
    
    triggers {
        pollSCM('H/5 * * * *')  //this make build every 5 mins
    }

    options {
        skipStagesAfterUnstable()
    }

    stages {
        
        stage('Install Chrome') {
            steps {
                sh '''
                sudo apt-get update
                sudo apt-get install -y google-chrome-stable
                '''
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Run tests') {
            environment {
                CHROMEWEBDRIVER = '/usr/bin/google-chrome'
            }
            steps {
                sh 'dotnet test'
            }
        }
    }
}
