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
    git branch: 'anchore-branch', credentialsId: '957b543e-6f77-4cef-9aec-82e9b0230975', url: 'https://github.com/devopstrainingblr/maven-web-application-1.git'
	
	}
  }
  
  stage('Build'){
  steps{
  sh  "mvn clean package"
  }
  }
	stage('Docker-Scanner'){
	steps{
		script{
			def imageLine = 'openjdk:8-jre-alpine'
			writeFile file: 'anchore_images', text: imageLine
			anchore name: 'anchore_images'
		}
	}
	}
			
}//Stages Closing
}//Pipeline closing
