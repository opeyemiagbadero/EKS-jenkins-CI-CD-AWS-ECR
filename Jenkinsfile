#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }

     environment {
        DOCKER_SERVER = "690769870672.dkr.ecr.eu-west-2.amazonaws.com"
        DOCKER_REPO = "${DOCKER_SERVER}/java-maven-app"
    }
    stages {
        stage("increment Version") {
            steps {
                script {
                    echo 'incrementing version'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }

        stage("Build App") {
            steps {
                script {
                    echo 'building the app'
                    sh 'mvn clean package'
                }
            }
        }
        stage('Build Image') {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'ecr-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t ${DOCKER_REPO}:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin ${DOCKER_SERVER}"
                        sh "docker push ${DOCKER_REPO}:${IMAGE_NAME}"
                    }
                }
            }
        }

        stage('Deploy_Env') {
            environment{
                AWS_ACCESS_KEY_ID = credentials('Jenkins-AWS-ACCESS-KEY')
                AWS_SECRET_ACCESS_KEY = credentials('Jenkins-AWS-SECRET-ACCESS-KEY')
                APP_NAME = 'java-maven-app'
            }
            steps {
                script {
                    echo "Deploying Docker Image"
                    sh 'envsubst < kubernetes/deployment.yaml | kubectl apply -f -'
                    sh 'envsubst < kubernetes/service.yaml | kubectl apply -f -'
                   
                }
            }
        }
        stage('commit version update') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: '2busola-jenkins', keyFileVariable: 'ssh_key_credential')]) {
                        sh 'git config --global user.email "jenkins@example.com"'
                        sh 'git config --global user.name "jenkins"'

                        sh 'git status'
                        sh 'git branch'
                        
                        sh 'git remote set-url origin git@github.com:opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR.git'
                        sh 'git add .'
                        sh 'git commit -m "ci:version bump-confirm"'
                        sh 'git push origin HEAD:main'
                        
                    }
                }
            }
        }
        
    }

    post {
        success {
            echo 'Pipeline succeeded! Send notifications, etc.'
        }
        failure {
            echo 'Pipeline failed! Send notifications, etc.'
        }
    }
}