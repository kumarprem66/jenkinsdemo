pipeline {
    agent any

    tools {
        maven "maven"
    }

    stages {

        stage('SCM checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: 'main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/kumarprem66/jenkinsdemo.git']]
                )
            }
        }

        stage('Build Process') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Deploy to Container') {
            steps {
                deploy adapters: [
                    tomcat9(
                        alternativeDeploymentContext: '',
                        credentialsId: 'tomcat-pwd',
                        path: '',
                        url: 'http://localhost:9090/'
                    )
                ],
                contextPath: 'jenkinsCICD',
                war: '**/*.war'
            }
        }
    }

    post {
    always {
        emailext(
            attachLog: true,
            mimeType: 'text/html',
            subject: "Pipeline Status : ${currentBuild.currentResult}",
            to: 'premstart10@gmail.com',
            body: """
            <html>
            <body>
                <p>Build Status : ${currentBuild.currentResult}</p>
                <p>Build Number : ${BUILD_NUMBER}</p>
                <p>
                    Check the
                    <a href="${BUILD_URL}console">
                        console output
                    </a>
                </p>
            </body>
            </html>
            """
        )
    }
}
}