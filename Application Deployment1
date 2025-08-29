# Jenkins
# Application Deployment using pipeline in jenkins, tomcat, nexus(artifacts)
pipeline {
    agent {
        node 'dev'
    }
    tools {
       maven 'mymaven'
    }
    environment{
        SCANNER_HOME = "mysonar"
    }
    stages {
        stage('Code') {
            steps {
                git branch: 'develop-1.0', url: 'https://github.com/jyothiharsha21/demorepo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('CQA') {
            steps {
                sh '''
                mvn sonar:sonar \
                -Dsonar.projectKey=mydev \
                -Dsonar.host.url=http://13.60.58.139:9000 \
                -Dsonar.login=0c5d0b277c03ac243c0ee1f96bd4a6e9587a9eff
                '''
            }
        }
        stage('Artifact') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'myweb', classifier: '', file: 'target/myweb-8.6.9.war', type: 'war']], credentialsId: 'ID_Nexus', groupId: 'in.javahome', nexusUrl: '13.53.80.221:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'myrepo', version: '8.69'
            }
        }
        stage('deploy') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'ID_Tomcat', path: '', url: 'http://51.20.183.110:8080/')], contextPath: 'myweb', war: 'target/*.war'
            }
        }
    }
}
post {
    success{
        echo "Harsha you are success"
    }
}
