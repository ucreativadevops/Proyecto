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

        stage('Run SonarQube'){
            steps{
                withSonarQubeEnv('SonarQubeCursoCI') {
                    bat "sonar-scanner -Dsonar.projectKey=DotnetApp"
                }
            }
        }

        stage('SonarQube Quality Gate') {
            steps {
                sleep 5
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
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
                bat '''
                    sc stop DotnetAppService
                    rmdir C:\\deploy\\DotnetApp\\ /s /q
                    xcopy bin\\Release\\net7.0\\publish\\ C:\\deploy\\DotnetApp\\ /s /q /y
                    sc start DotnetAppService
                '''
            }
        }
    }
}