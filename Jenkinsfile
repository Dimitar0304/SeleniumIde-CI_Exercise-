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
                bat '''
                sudo apt-get update
                sudo apt-get install -y google-chrome-stable
                '''
            }
        }

        stage('Install dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build --no-restore'
            }
        }

        stage('Run tests') {
            environment {
                CHROMEWEBDRIVER = '/usr/bin/google-chrome'
            }
            steps {
                bat 'dotnet test'
            }
        }
    }
}
