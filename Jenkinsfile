
node {
  stage('SCM') {
    git 'https://github.com/ricardo-softinsa/get-started-node.git'
  }
  stage('SonarQube analysis') {
    // requires SonarQube Scanner 2.8+
    def scannerHome = tool 'Scanner';
    withSonarQubeEnv('SonarServer') {
      bat "${scannerHome}/bin/sonar-scanner"
    }
  }
}