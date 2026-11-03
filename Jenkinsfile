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
   }
}