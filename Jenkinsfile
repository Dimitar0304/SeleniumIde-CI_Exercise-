pipeline {
    agent {
        label 'ubuntu'
    }
    
    triggers {
        pollSCM('H/5 * * * *')  //this make build every 5 mins
    }

    options {
        skipStagesAfterUnstable()
    }

    stages {
        

        stage('Set up .NET Core') {
            steps {
                script {
                    
                    sh 'sudo apt-get update'
                    sh 'sudo apt-get install -y apt-transport-https'
                    sh 'wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb'
                    sh 'sudo dpkg -i packages-microsoft-prod.deb'
                    sh 'sudo apt-get update && sudo apt-get install -y dotnet-sdk-6.0'
                }
            }
        }

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
