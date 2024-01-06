## EKS-jenkins-CI-CD-AWS-ECR
This is an overview of how to deploy into a kubernetes cluster from a Jenkins CI/CD through AWS ECR. The prerequisites of the project include the installation of Jenkins (from a docker image) on a container, setup of the jenkins server, docker, AWS cli, kubectl, AWS Iam authenticator and AWS credentials using jenkins. 

Created ECR Repository
❏Created Credential for ECR repository in Jenkins
❏Created Secret for AWS ECR Registry in EKS cluster and adjusted
reference in Deployment ﬁle
❏Updated Jenkinsﬁle
❏Executed Jenkins Pipeline
