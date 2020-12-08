pipeline {
    agent {
      label 'slave-node'
    }
    stages {
        stage('Build') {
            steps {
                powershell 'dotnet restore'
                powershell 'dotnet sonarscanner begin /k:"dotnet-key" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="b410f253da4c8a13463b34fb44d6a179e570ab7c" /d:sonar.cs.opencover.reportsPaths=".\\tests\\Conduit.IntegrationTests\\coverage.opencover.xml" /d:sonar.cs.vstest.reportsPaths=".\\estResults\\*.trx"'
                powershell 'dotnet build'
                powershell 'dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover --collect:"Code Coverage" --logger trx --results-directory "TestResults"'
                powershell 'dotnet sonarscanner end /d:sonar.login="b410f253da4c8a13463b34fb44d6a179e570ab7c"'
            }
        }
        stage('Pack') {
            steps {
                powershell 'dotnet publish .\\Conduit.sln -c Release -o published\\lib --runtime win81-x64'
                powershell 'dotnet pack .\\src\\Conduit\\Conduit.csproj'
            }
        }
        stage('Deploy') {
            steps {
                powershell 'dotnet nuget push .\\src\\Conduit\\bin\\Debug\\Conduit.*.nupkg -k 5a46c671-b21a-3810-9b29-c8913bfc4ae4 -s http://localhost:8081/repository/nuget-hosted/'
            }
        }
    }
}