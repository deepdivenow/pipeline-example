node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git ([url: 'https://github.com/deepdivenow/pipeline-example.git', branch: 'master'])
      mvnHome = tool 'localMaven'
   }
   stage('Build') {
      // Run the maven build
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
   }
   stage('Test CodeStyle'){
        // Run the maven codestyle
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore checkstyle:checkstyle"
         checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
   }
   stage('Store Package') {
      archiveArtifacts '**/*.war'
   }
   stage('Deploy') {
      // Deploy package to dev
   }
}