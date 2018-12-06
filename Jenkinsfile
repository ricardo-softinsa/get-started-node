node {
  stage('SCM') {
    git 'https://github.com/ricardo-softinsa/get-started-node.git'
  }
  stage('Node Version'){
	bat 'php -v'
  }
  stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'Scanner';
    withSonarQubeEnv('SonarServer') {
      bat "\"${scannerHome}/bin/sonar-scanner\""
    }
  }
  stage("SonarQube Quality Gate") { 
	timeout(time: 2, unit: 'MINUTES') { 
	   def qg = waitForQualityGate() 
	   if(qg.status == "ERROR"){
			echo "Failed Quality Gates";
	   }
	   if (qg.status == 'OK') {
		 echo "Passed Quality Gates!";
	   }
	   
	}
  }
}