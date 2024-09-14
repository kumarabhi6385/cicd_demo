pipeline {
    agent any

    environment {
        buildConfiguration = 'Release'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Install .NET SDK') {
            steps {
                script {
                    def dotnetSdkVersion = '8.x'
                    sh "wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh"
                    sh "chmod +x dotnet-install.sh"
                    sh "./dotnet-install.sh --version ${dotnetSdkVersion} --install-dir ${env.HOME}/.dotnet"
                    env.PATH = "${env.HOME}/.dotnet:${env.PATH}"
                }
            }
        }

        stage('Build Project') {
            steps {
                script {
                    echo "Current directory: ${pwd()}"
                    dir('helloworld') {
                        sh "dotnet build --configuration ${buildConfiguration}"
                    }
                }
            }
        }

        stage('Publish Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/bin/**', allowEmptyArchive: true
            }
        }
    }
}
