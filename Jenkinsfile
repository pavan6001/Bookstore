pipeline {
	agent any
	tools {
	  maven 'MAVEN'
	}
	stages {
	  stage ('Initialize') {
	    steps {
	      sh '''
					echo "PATH = ${PATH}"
					echo "M2_HOME = ${M2_HOME}"
			 '''
			}
		}
		
	  stage ('Check-git-secrets') {
		steps {
				sh 'rm trufflehog || true'
				sh 'docker run gesellix/trufflehog --json https://github.com/pavan6001/Bookstore.git > trufflehog'
				sh 'cat trufflehog'
			}
		}	
		
	  stage ('Build') {
		steps {
			sh 'mvn clean package'
			}
		}
	
      stage ('Deploy to Tomcat') {
		steps {
			sshagent(['tomcat']) {
					sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.127.237.80:/opt/apache-tomcat-8.5.57/webapps/*.war'
				}
		    }
        }	  
	}
}
