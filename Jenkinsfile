pipeline {
    agent {
      label 'slave-node'
    }
    stages {
        stage('Test') {
            steps {
                bat 'dotnet sonarscanner begin /k:"dotnet-key" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="b410f253da4c8a13463b34fb44d6a179e570ab7c"'
                bat 'dotnet test'
                bat 'dotnet sonarscanner end /d:sonar.login="b410f253da4c8a13463b34fb44d6a179e570ab7c"'
            }
        }
        stage('Build') {
            steps {
                bat 'dotnet build'
            }
        }
        stage('Pack') {
            steps {
                bat 'dotnet pack'
            }
        }
        stage('Deploy') {
            steps {
                bat 'dotnet nuget push build/bin/Debug/build.1.0.0.nupkg -k 5a46c671-b21a-3810-9b29-c8913bfc4ae4 -s http://localhost:8081/repository/nuget-hosted/'
            }
        }
    }
}