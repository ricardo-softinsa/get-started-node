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

	slackMet.call(currentBuild.currentResult);
  }
  stage("Check App Status"){
	echo "Checking if the App is live..."
	def slackMet = load("slackNotifications.groovy");
	try{
		bat "curl -s --head  --request GET https://node-softinsa-app.eu-gb.mybluemix.net/ | grep '200 OK'"
		echo "The app is up and running!"
		slackMet.isRunning("Running");
	}catch(e){
		echo "The app is down..."
		slackMet.isRunning("NotRunning");
	}
  }
}
