## EKS-jenkins-CI-CD-AWS-ECR
This is an overview of how to deploy into a kubernetes cluster from a Jenkins CI/CD through AWS ECR. The prerequisites of the project include the installation of Jenkins (from a docker image) on a container, setup of the jenkins server, installation of docker, AWS cli, kubectl, AWS Iam authenticator and AWS credentials inside the jenkins container. The same deployment and service yaml files used in the  EKS-jenkins-CI-CD-AWS-docker project were used in this project also

Created an ECR in AWS called java-mave-app

![1  created an ECR for the application on AWS](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/428af01a-b20f-471a-b5eb-7e9517eb0bd7)

Created Credential for ECR repository in Jenkins

![2  ECR credentials on jenkins](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/3fba0872-3cd0-4e8f-ae7d-a73a982877bb)

Created Secret for AWS ECR Registry in EKS cluster and adjusted reference in deployment ﬁle to fecth the image from AWS ECR 



![4  edit the deployment file to input the name of the secret](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/b5b124ea-98e0-4e34-9dbe-81e56cb2ee99)

Updated Jenkinsﬁle used in the EKS-jenkins-CI-CD-AWS-docker to set environment variables with envsubst.



❏Executed Jenkins Pipeline
