pipeline {
  agent any
  stages {
    stage('Static-analysis') {
      steps {
        echo 'Static code Analysis'
        //checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/ajit-t-5144/DevOps-Demo-WebApp.git']]])
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
    
  } // Stage 1 complete 
  
  stages{
    
    // Start of Parallel Step 
    parallel {
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
   
    } // End of Parallel
    
  } // Stage 2 complete 
  
  stages{
    
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

  }// Stage 3 complete 
}
