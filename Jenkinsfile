pipeline {
    agent any
     
    environment {
        AWS_ACCESS_KEY_ID = credentials('abullido481-aws-key')
	AWS_SECRET_ACCESS_KEY = credentials('abullido481-aws-pass')
    }

    stages {
        stage ("Terraform init") {
            steps {
                sh "terraform init"
            }
        }

        stage ("Terraform import") {
            steps {
                sh "terraform import aws_s3_bucket.bucket mapfre-gitops-abullido481
            }
        }

        stage ("Terraform plan") {
            steps {
                sh "terraform plan"
            }
        }

        stage ("Terraform apply") {
            when {
                anyOf {
                    branch 'main'
                }
            }
            steps {
                sh "terraform apply --auto-approve"
            }
        }

        stage ("Ansible test") {
            steps {
                sh "ansible-playbook reto-s3.yml --check"
            }
        }

        stage ("Ansible apply") {
            when {
                anyOf {
                    branch 'main'
                }
            }
            steps {
                sh "ansible-playbook reto-s3.yml"
            }
        }
    }
}

