pipeline {
  agent any
  //define artifactserver = artifactory.server('ajdevopstcs1.jfrog.io')
  
  //Global tools 
  tools { 
        maven 'maven' 
        jdk  'jdk'
    }
  
  // Job stages
  stages {
    
    // Static code analysis - inject sonarqube 
    stage('Static-analysis') {
      steps {
        echo 'Static code Analysis'
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/ajit-t-5144/DevOps-Demo-WebApp.git']]])
        withSonarQubeEnv(credentialsId: 'sonar', installationName: 'sonarqube')
           {sh 'mvn clean compile sonar:sonar -Dsonar.host.url=http://13.64.108.228:9000 -Dsonar.sources=. -Dsonar.tests=. -Dsonar.inclusions=**/test/java/servlet/createpage_junit.java -Dsonar.test.exclusions=**/test/java/servlet/createpage_junit.java -Dsonar.login=admin -Dsonar.password=admin' 
            }
        slackSend channel: '#devops', message: 'Stattic test analysis completed'
      }
    } // Stage end

    
   //Compile the Web app 
    stage('Compile-Build') {
      steps {
        echo 'Build the code'
        sh 'mvn clean compile'
      }
    }
  
    //Deploy the application to tomcat Test Server 
    stage('Deploy To Test') {
      steps {
        echo 'Deploy to Test'
        sh 'mvn clean package'
        deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://13.82.213.187:8080')], contextPath: '/QAWebapp', war: '**/*.war'
        slackSend channel: '#devops', message: 'Code deployed to Test Server'
      }
    }
    
    //Store artifact and Build info in artifactory server
    stage('Store Artifact') {
      steps {
        echo 'Store Artifact' 
        rtUpload(serverId: 'artifactory')
        rtPublishBuildInfo (serverId: 'artifactory')
      }
    }

    //Perform UI Test and Publish the report
    stage('Perform UI Test') {
      steps {
        echo 'UI Test'
        sh 'mvn test -f functionaltest/pom.xml'
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\functionaltest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'UI-Test', reportTitles: ''])
      }
    }
   
    //Run Performance Test using Blazemeter
    stage('Performance Test') {
      steps {
        echo 'Performance test'
        //blazeMeterTest(credentialsId: 'blazemeter', workspaceId: '680689', testId: '8642591.taurus')
      }
    }

    //Deploy the Webapp to Production tomcat Server 
    stage('Deploy to Production') {
      steps {
        echo 'Deploy to Production'
        sh 'mvn clean install'
        deploy adapters: [tomcat8(credentialsId: 'tomcat', path: '', url: 'http://13.82.211.112:8080')], contextPath: '/ProdWebapp', war: '**/*.war'
        slackSend channel: '#devops', message: 'Code deployed to prod server'
      }
    }
    
    //Run Sanity Check and publish the report  
    stage('Perform Sanity Check') {
      steps {
        echo 'Perform Sanity Check'
        sh 'mvn test'
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '\\Acceptancetest\\target\\surefire-reports', reportFiles: 'index.html', reportName: 'Sanity Test report', reportTitles: ''])
        slackSend channel: '#devops', message: 'Sanity test completed successfully'
      }
    }

  } //Job stages end 
} // end of Pipeline
