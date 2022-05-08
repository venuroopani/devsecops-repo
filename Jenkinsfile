pipeline{

agent any
triggers{
pollSCM('* * * * *')
}

options{
timestamps()
buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
}

stages{

  stage('CheckOutCode'){
    steps{
    git branch: 'development', credentialsId: '957b543e-6f77-4cef-9aec-82e9b0230975', url: 'https://github.com/devopstrainingblr/maven-web-application-1.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }
 stage('SCA'){
  steps{
	  script{
	dependencyCheck additionalArguments: '', odcInstallation: 'sca'
	
  }
  }
  }
//  stage('SCA-RESULTS-CHECK'){
//  steps{
//	  script{
//	dependencyCheckPublisher failedTotalCritical: 1, failedTotalHigh: 1, failedTotalLow: 1, failedTotalMedium: 1, pattern: '**/dependency-check-report.xml'
//  	if (currentBuild.rawBuild.getLog(50).contains('[DependencyCheck] Findings exceed configured thresholds')) {
//        error("Build failed due to vulnerabilities found during dependencyCheck")    
//}else{
//        sh 'echo "No vulnerabilities found during dependencyCheck"'
//} 
//	  } 
//  }
//  }
        stage('SAST-SONARQUBE') {
          steps {
            withSonarQubeEnv('sonarcloud') {
               sh '''${scannerHome}/bin/sonar-scanner \
	       	   -Dsonar.organization=devsecops-sast \
	       	   -Dsonar.projectKey=sast-java-key \
                   -Dsonar.projectName=sast-java \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/ '''
            }

            timeout(time: 20, unit: 'SECONDS') {
               waitForQualityGate abortPipeline: true
            }
          }
        }
 /* stage('UploadArtifactsIntoNexus'){
  steps{
  sh  "mvn clean deploy"
  }
  }
  
  stage('DeployAppIntoTomcat'){
  steps{
  sshagent(['bfe1b3c1-c29b-4a4d-b97a-c068b7748cd0']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.190.162:/opt/apache-tomcat-9.0.50/webapps/"    
  }
  }
  }
  */
}//Stages Closing


}//Pipeline closing
