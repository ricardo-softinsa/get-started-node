node {
  stage('SCM') {
	git 'https://github.com/ricardo-softinsa/get-started-node'
  }
  stage('SonarQube analysis') {
	// requires SonarQube Scanner 2.8+
	def scannerHome = tool 'SonarScanner';
	withSonarQubeEnv('Sonar') {
	  sh """ ${scannerHome}/bin/sonar-scanner -D sonar.login=admin -D sonar.password=admin """
	}
  }
  stage("SonarQube Quality Gate") { 
	timeout(time: 1, unit: 'HOURS') { 
	   def qg = waitForQualityGate() 
	   if (qg.status != 'OK') {
		 error "Pipeline aborted due to quality gate failure: ${qg.status}"
	   }
	}
  }
}
