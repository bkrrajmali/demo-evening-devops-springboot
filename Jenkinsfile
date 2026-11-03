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
   }
}