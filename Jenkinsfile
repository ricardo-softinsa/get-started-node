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
}