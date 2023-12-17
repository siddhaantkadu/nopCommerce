pipeline {
    agent { label 'DOTNET'}
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('git') {
            steps {
                git url: 'https://github.com/siddhaantkadu/nopCommerce.git',
                    branch: 'develop'
            }
        }
        stage('Build') {
            steps {
                sh 'dotnet build -c Release src/NopCommerce.sln'
                sh 'dotnet publish -c Release src/Presentation/Nop.Web/Nop.Web.csproj  -o "./published"'
            }
            post {
                success {
                    sh 'mkdir -p ./published/bin ./published/logs'
                    sh 'tar -czvf nop.web.tar.gz ./published/'
                    archiveArtifacts artifacts: '**/*.tar.gz'
                }
                failure {
                    mail subject: '$JOB_BASE_NAME Failed',
                         from: 'siddhant.hemant.kadu@vz.com',
                         to: 'vzi-devops@vz.com',
                         body: 'Refer Here for Build URL $BUILD_URL'
                }
            }
        }
    }
}