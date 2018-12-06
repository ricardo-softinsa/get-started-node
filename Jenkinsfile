node {
  stage('SCM') {
  echo "One";
    git 'https://github.com/ricardo-softinsa/get-started-node.git'
	echo "Two";
  }
  stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
	echo "Three";
    def scannerHome = tool 'Scanner';
	echo scannerHome;
	echo "\"${scannerHome}/bin/sonar-scanner\"";
	echo "Four";
    withSonarQubeEnv('SonarServer') {
		echo "Five";
      bat "\"${scannerHome}/bin/sonar-scanner\""
	  echo "Six";
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