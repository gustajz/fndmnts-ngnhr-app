pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                withDockerContainer(args: '-e MAVEN_OPTS="-Dmaven.repo.local=.m2/repository -Dorg.slf4j.simpleLogger.log.org.apache.maven.cli.transfer.Slf4jMavenTransferListener=WARN -Dorg.slf4j.simpleLogger.showDateTime=true -Djava.awt.headless=true"', image: 'maven:3.5-jdk-8') {
                    def MAVEN_CLI_OPTS = "--batch-mode --errors --fail-at-end --show-version -DinstallAtEnd=true -DdeployAtEnd=true"
                    sh "mvn $MAVEN_CLI_OPTS clean package -Dmaven.test.skip=true"
                }
            }
        }
        stage('Push') {
            steps {
                dir('target') {
                    sh "docker build -t registry.172.25.56.1.nip.io:5000/fndmnts-ngnhr-app:${env.BUILD_ID}"
                    sh "docker push registry.172.25.56.1.nip.io:5000/fndmnts-ngnhr-app:${env.BUILD_ID}"
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
