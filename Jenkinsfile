
node {
  stage('SCM') {
    git 'https://github.com/ricardo-softinsa/get-started-node.git'
  }
  stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'Sonar-Scanner';
    withSonarQubeEnv('Sonar-Server') {
      sh "${scannerHome}/bin/sonar-scanner"
    }
  }
}