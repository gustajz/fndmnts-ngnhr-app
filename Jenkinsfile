pipeline {
    agent {
        docker {
            image 'maven:3.5-jdk-8'
            args '-e MAVEN_OPTS="-Dmaven.repo.local=.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true"'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -Dmaven.test.skip=true'
            }
        }
        stage('Push') {
            steps {
                dir('target') {
                    docker.withRegistry('http://registry.172.25.56.1.nip.io:5000') {
                        def customImage = docker.build("fndmnts-ngnhr-app:${env.BUILD_ID}")
                        customImage.push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                dir('target') {
                    withKubeConfig(credentialsId: 'KUBECONIG') {
                        sh 'kubectl get no'
                    }
                }
            }
        }
    }
}
