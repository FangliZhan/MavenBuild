node('') {
	stage ('checkout code'){
		checkout scm
	}
	
	stage ('Build'){
		sh "mvn clean install -Dmaven.test.skip=true"
	}

	stage ('Test Cases Execution'){
		sh "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent install -Pcoverage-per-test"
	}

	stage ('Archive Artifacts'){
		archiveArtifacts artifacts: 'target/*.war'
	}
	
	stage ('Deployment'){
		deploy adapters: [tomcat9(credentialsId: 'tomcatCreds', path: '', url: 'http://44.206.183.176:8081/')], contextPath: 'counterwebapp', war: 'target/*.war'
	}
	
	stage ('Notification'){
		slackSend channel: '#cicd-automation', 
                          message: 'slack-notification: deployment is complete'
	}
}
