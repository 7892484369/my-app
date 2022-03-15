pipeline {
  agent any
  tools {
    maven 'maven3'
  }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '10', numToKeepStr: '7')
  }
  parameters {
    choice choices: ['develop', 'qa', 'master'], description: 'Choose the branch to Build ', name: 'branchName'
  }
  stages {
    stage("Git Clone") {
      steps {
        git branch: 'develop', url: 'https://github.com/7892484369/Devops-2020'
      }
    }
    stage('Maven Buuild') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Deploy to Tomcat') {
      steps {
        sshagent(['tomcat-dev']) {
          sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.29.148:/opt/tomcat/webapps/app.war"
          sh "ssh ec2-user@172.31.29.148 /opt/tomcat/bin/shutdown.sh"
          sh "ssh ec2-user@172.31.29.148 /opt/tomcat/bin/startup.sh"
        }

      }
    }
  }
  post {
    success {
      archiveArtifacts artifacts: 'target/*.war'
      cleanWs()
    }
  }
}
