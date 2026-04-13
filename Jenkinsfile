pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build + Tests + Coverage') {
            steps {
                sh 'mvn clean verify -B'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                    junit '**/target/failsafe-reports/*.xml'
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Qualite statique') {
            steps {
                sh 'mvn checkstyle:checkstyle pmd:pmd pmd:cpd spotbugs:spotbugs -B'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'target/checkstyle-result.xml, target/pmd.xml, target/cpd.xml, target/spotbugsXml.xml', allowEmptyArchive: true
                }
            }
            post {
                failure {
                    emailext (
                        subject: "Build FAILED",
                        body: "Le build a échoué",
                        to: "dydoudubg@gmail.com"
                    )
                }
            }
        }
    }
}