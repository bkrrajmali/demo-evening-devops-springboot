pipeline {
    agent any
    tools {
        maven 'maven'
    }



stages {
    stage ('Checkout FROM Git') {
        steps {
            git branch: 'master', url:'https://github.com/bkrrajmali/demo-evening-devops-springboot.git'
        }
    }
    stage ('maven validate') {
        steps {
            sh 'mvn validate'
        }
    }
    stage ('maven compile') {
        steps {
            sh 'mvn compile'
        }
     }
     stage ('maven test') {
        steps {
            sh 'mvn test'
        }
    }
    stage ('maven package') {
        steps {
            sh 'mvn package'
        }
      }
      stage ('Sonar Analysis') {
        environment {
            SCANNER_HOME = tool 'sonar-scanner'
        }
        steps {
            withSonarQubeEnv('sonarserver') {
                sh ''' ${SCANNER_HOME}/bin/sonar-scanner \
                -Dsonar.organization=bkrrajmali \
                -Dsonar.projectKey=myspringbootapp \
                -Dsonar.projectName=springbootapp \
                -Dsonar.sources=src/main/java \
                -Dsonar.java.binaries=target/classes
                '''
            }
        }
      }
      stage ('Docker Build') {
        steps {
            script {
              sh 'docker build -t petclinic:v1 .'
              
            }
        }
      }
      stage ('Login to AWS and Push Image to ECR') {
        steps {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-jenkins']]) {
                sh '''
                aws ecr get-login-password --region ap-south-1 | \
                docker login --username AWS --password-stdin 175157388210.dkr.ecr.ap-south-1.amazonaws.com
                docker tag petclinic:v1 175157388210.dkr.ecr.ap-south-1.amazonaws.com/petclinic:latest
                docker push 175157388210.dkr.ecr.ap-south-1.amazonaws.com/petclinic:latest
                '''
            }
        }
      }
      stage ('Deploy To EKS') {
        steps {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-jenkins']]) {
                sh '''
                aws eks update-kubeconfig --region ap-south-1 --name practice-eks
                kubectl apply -f k8s/sprinboot-deployment.yaml
                '''
            }
         }
      }
   }
}