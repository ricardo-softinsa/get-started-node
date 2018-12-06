node {
  stage('SCM') {
	git 'https://github.com/ricardo-softinsa/get-started-node'
  }
  stage('SonarQube analysis') {
	// requires SonarQube Scanner 2.8+
	def scannerHome = tool 'SonarQube Scanner 3.2.0.1227';
	withSonarQubeEnv('Sonar') {
	  bat "${scannerHome}/bin/sonar-scanner"
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
