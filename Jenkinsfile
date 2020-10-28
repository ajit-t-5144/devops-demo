pipeline {
  agent any
  tools { 
        maven 'maven' 
        jdk  'jdk'
    }
  stages {
    stage('Static-analysis') {
      steps {
        echo 'Static code Analysis'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/ajit-t-5144/DevOps-Demo-WebApp.git']]])
        //waitForQualityGate(abortPipeline: true, credentialsId: 'sonarqube', installationName: 'sonarqube')
        withSonarQubeEnv(credentialsId: 'sonarqube', installationName: 'sonarqube')
           {    withMaven{
                  //sh 'maven $SONAR_MAVEN_GOAL -Dsonar.host.url=$SONAR_HOST_URL'
                  //sh 'mvn -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=admin -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.sources=. sonar:sonar -Dsonar.host.url=http://35.193.147.208:9000'
                  sh 'mvn clean package sonar:sonar -Dsonar.host.url=http://URL:9000 -Dsonar.sources=. -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=admin' 
                  }
                }
      }
    }

    stage('Compile-Build') {
      steps {
        echo 'Build the code'
      }
    }

    stage('Deploy To Test') {
      steps {
        echo 'Deploy to Test'
      }
    }

    stage('Store Artifact') {
      steps {
        echo 'Store Artifact'
      }
    }

    stage('Perform UI Test') {
      steps {
        echo 'UI Test'
      }
    }

    stage('Performance Test') {
      steps {
        echo 'Performance test'
      }
    }

    stage('Deploy to Production') {
      steps {
        echo 'Deploy to Production'
      }
    }

    stage('Perform Sanity Check') {
      steps {
        echo 'Perform Sanity Check'
      }
    }

  }
}
