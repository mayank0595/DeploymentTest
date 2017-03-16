node {
   def version
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/mayank0595/DeploymentTest.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'LOCAL_MAVEN'
      version = '2.3.5'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
   }
   stage('Publish') {
     nexusPublisher nexusInstanceId: 'NEXUS', nexusRepositoryId: 'QARelease', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: 'addressbook_main/target/addressbook.war']], mavenCoordinate: [artifactId: 'addressbook_main', groupId: 'com.edurekademo.tutorial', packaging: 'war', version: '2.3.0']]]
   }
}
