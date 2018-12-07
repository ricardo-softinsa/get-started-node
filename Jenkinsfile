node {
  stage('SCM') {
    git 'https://github.com/ricardo-softinsa/get-started-node.git'
  }
  stage('SonarQube analysis') {
    def scannerHome = tool 'Scanner';
    withSonarQubeEnv('SonarServer') {
      bat "\"${scannerHome}/bin/sonar-scanner\""
    }
  }
  stage("SonarQube Quality Gate") { 
	def slackMet = load("slackNotifications.groovy");
	timeout(time: 2, unit: 'MINUTES') { 
	   def qg = waitForQualityGate() 
	   if(qg.status == "ERROR"){
		echo "Failed Quality Gates";
		slackMet.afterQG(qg.status);
		error "Pipeline aborted due to quality gate failure: ${qg.status}"
		waitForQualityGate abortPipeline: true
	   }
	   if (qg.status == 'OK') {
		 echo "Passed Quality Gates!";
		 slackMet.afterQG(qg.status);
	   }
	}
  }
  stage("Pushing to Cloud"){
	def slackMet = load("slackNotifications.groovy");
	echo "Pushing into the cloud...";
	cfPush(
		target: 'api.eu-gb.bluemix.net',
    		organization: 'ricardo.miguel.magalhaes@pt.softinsa.com',
    		cloudSpace: 'dev',
		credentialsId: 'CFPush',
	)
	slackMet.call(currentBuild.currentResult);
  }
}
