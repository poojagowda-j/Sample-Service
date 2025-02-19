@Library('java_demo_pipeline@main') _
pipeline {
    agent {
        label 'slave3'
    }
    stages {
        stage('Checkout') {
            steps {
                 sh "rm -rf Sample-Service"
                 sh "git clone https://github.com/poojagowda-j/Sample-Service.git"
                sh "cd Sample-Service"
                //checkoutcode('parcel')
            }
        }
        stage('Set up Environment') {
            steps {
                sh 'export export JAVA_HOME=$(dirname $(dirname $(readlink -f $(which java))))'
                sh 'export MAVEN_HOME=/usr/share/maven'
            }
        }
        stage('setupjava17') {
            steps {
                setupjava('openjdk-17-jdk')
            }
        }
        stage('setupmaven') {
            steps {
                //   echo " installing maveen"
                //sh "sudo apt install -y maven"
                setupjava('maven')
            }
        }
        stage('build') {
            steps {
                // sh "mvn clean package"
                buildproject(sample-service)
            }
        }
        stage('Upload Artifact') {
            steps {
                echo 'Uploading artifact...'
                archiveArtifacts artifacts: 'target/simple-parcel-service-app-1.0-SNAPSHOT.jar', allowEmptyArchive: true
            }
        }
        stage('Run Application') {
            steps {
                echo 'Running Spring Boot application...'
                sh 'mvn spring-boot:run '
            }
        }
    }
}
