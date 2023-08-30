def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
    'UNSTABLE': 'danger'
]
pipeline {
  agent {
    label 'Maven-Build-Env' // Use the Maven slave node for this pipeline
  }
  stages {
    stage('Validate Project') {
        steps {
            sh 'mvn validate'
        }
    }
    stage('Unit Test'){
        steps {
            sh 'mvn test'
        }
    }
    stage('Integration Test'){
        steps {
            sh 'mvn verify -DskipUnitTests'
        }
    }
    stage('App Packaging'){
        steps {
            sh 'mvn package'
        }
    }
    stage ('Checkstyle Code Analysis'){
        steps {
            sh 'mvn checkstyle:checkstyle'
        }
    }
    stage('SonarQube Inspection') {
        steps {
            sh  """mvn sonar:sonar \
                   -Dsonar.projectKey=Maven-JavaWebApp \
                   -Dsonar.host.url=http://172.31.12.227:9000 \
                   -Dsonar.login=a040d936f00ce67e8bdf21ee2cecd6e237ddb236"""
        }
    }
    stage("Upload Artifact To Nexus"){
        steps{
             sh 'mvn deploy'
        }
        post {
            success {
              echo 'Successfully Uploaded Artifact to Nexus Artifactory'
        }
      }
    }
  }
}
