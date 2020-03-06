pipeline {
    agent any 
    tools { 
        maven 'maven' 
      
    }
stages { 
     
 stage('Preparation') { 
     steps {
// for display purpose

      // Get some code from a GitHub repository

      git 'https://github.com/Jyothi1201/game-of-life.git'
     
         // Get the Maven tool.
     
 // ** NOTE: This 'M3' Maven tool must be configured
 
     // **       in the global configuration.   
     }
   }

   stage('Build') {
       steps {
       //Run the maven build

      //if (isUnix()) {
         sh 'mvn -Dmaven.test.failure.ignore=true install'
      //} 
      //else {
      //   bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
       }
//}
   }
 
  stage('Unit Test Results') {
      steps {
      junit '**/target/surefire-reports/TEST-*.xml'
      
      }
 }
stage('Sonarqube') {
   environment {
       scannerHome = tool 'sonarqube'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
       }
    }
}
     stage('Artifact upload') {
      steps {
       nexusPublisher nexusInstanceId: '123', nexusRepositoryId: 'releases', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'gameoflife-web/target/gameoflife.war']], mavenCoordinate: [artifactId: 'gameoflife', groupId: 'com.wakaleo.gameoflife', packaging: 'war', version: '$BUILD_NUMBER']]]      
      }
     }
   // stage('Deploy War') {
     // steps {
       // sh label: '', script: 'ansible-playbook deploy.yml'
          //deploy adapters: [tomcat8(credentialsId: '0012', path: '', url: 'http://3.15.230.111:8080/manager/html')], contextPath: 'goldeploy', war: '**/*.war'
     // }
 //}
}
//post {
        //success {
            //archiveArtifacts 'gameoflife-web/target/*.war'
        }
       // failure {
         //   mail to:"sankar.dadi@qentelli.com", subject:"FAILURE: ${currentBuild.fullDisplayName}", body: "Build failed"
        //}
    //}       
}
