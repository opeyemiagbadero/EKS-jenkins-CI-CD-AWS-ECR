## EKS-jenkins-CI-CD-AWS-ECR
This is an overview of how to deploy into a kubernetes cluster from a Jenkins CI/CD through AWS ECR. The prerequisites of the project include the installation of Jenkins (from a docker image) on a container, setup of the jenkins server, installation of docker, AWS cli, kubectl, AWS Iam authenticator and AWS credentials inside the jenkins container. The same deployment and service yaml files used in the  EKS-jenkins-CI-CD-AWS-docker project were used in this project also

Created an ECR in AWS called java-mave-app

![1  created an ECR for the application on AWS](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/428af01a-b20f-471a-b5eb-7e9517eb0bd7)

Created Credential for ECR repository in Jenkins

![2  ECR credentials on jenkins](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/3fba0872-3cd0-4e8f-ae7d-a73a982877bb)

Created Secret for AWS ECR Registry in EKS cluster and adjusted reference in deployment ﬁle to fecth the image from AWS ECR 

![3  secret used in deployment stage to fetch  the image from AWS ECR](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/5e452448-d7b4-435d-8b3a-029754205bcb)

![4  edit the deployment file to input the name of the secret](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/b5b124ea-98e0-4e34-9dbe-81e56cb2ee99)

Updated Jenkinsﬁle used in the EKS-jenkins-CI-CD-AWS-docker to set environment variables with envsubst.

![9  Adjusted Jenkinsﬁle as used in the EKS-cluster-deployment-using-Jenkins repo to set environment variables with envsubst

editted the Jenkinsfile to reflect the envsubst and the new environmental variables](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/746eaa21-0bf0-4ad0-8f37-97c802836f0d)

Executed Jenkins Pipeline

![5  Running the pipeline succesfully](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/ca1bc59c-ba43-4da8-b64f-0fc582bb88b3)

Confirmed that the pods were running succefully

![6  pods running succesfully](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/a9d7096d-0ee2-42df-99da-6bc2c326e871)

![7  describe the running pod](https://github.com/opeyemiagbadero/EKS-jenkins-CI-CD-AWS-ECR/assets/79456052/329ef44f-30f8-48aa-988f-baeb5e473f8d)


