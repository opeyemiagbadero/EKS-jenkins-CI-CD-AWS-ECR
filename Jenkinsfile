#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.6'
    }

    stages {
        stage('increment version') {
            steps {
                script {
                    echo 'incrementing the app version...'
                    sh 'mvn build-helper:parse-version versions:set ' +
                       '-DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} ' +
                       'versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }

        stage('Build app') {
            steps {
                script {
                    echo 'Building the application...'
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build image') {
            environment {
                DOCKER_REPO_SERVER = '690769870672.dkr.ecr.eu-west-2.amazonaws.com'
                DOCKER_REPO = '${DOCKER_REPO_SERVER}/java-maven-app'
            }
            steps {
                script {
                    echo 'Building the Docker image...'
                    withCredentials([usernamePassword(credentialsId: 'ecr-credentials', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                        sh """
                            docker build -t ${DOCKER_REPO}:${IMAGE_NAME} .
                            echo \${PASSWORD} | docker login -u \${USERNAME} --password-stdin \${DOCKER_REPO_SERVER}
                            docker push ${DOCKER_REPO}:${IMAGE_NAME}
                        """
                    }
                }
            }
        }

        stage('Deploy') {
            environment {
                AWS_ACCESS_KEY_ID = credentials ('JENKINS-ACCESS-KEY-ID')
                AWS_SECRET_ACCESS_KEY =credentials ('JENKINS-SECRET-ACCESS-KEY')
                APP_NAME = 'java-maven-app'
            }
            steps {
                script {
                    echo 'Deploying docker image ...'
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