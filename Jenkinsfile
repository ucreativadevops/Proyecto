pipeline {
    agent {
        label 'windows-agent'
    }

    stages {
        stage('Dependencies'){
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Test'){
            steps {
                bat 'dotnet test'
            }
        }

        stage('Build'){
            steps {
                bat 'dotnet build'
            }
        }

        stage('Package'){
            steps {
                bat 'dotnet publish -c Release'
            }
        }

        stage('Deploy'){
            steps {
                bat 'xcopy bin\\Release\\net7.0\\publish\\ C:\\deploy\\DotnetApp\\ /s /q /y'
            }
        }
    }
}