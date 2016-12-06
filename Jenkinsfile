node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git url: 'https://github.com/khmelevskikh/blue_ocean.git', branch: 'testing'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'apache-maven-3.0.4-nc'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean install pmd:pmd"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean install pmd:pmd checkstyle:checkstyle/)
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('CAT') {
       parallel (
        "CheckStyle" : { step([$class: 'hudson.plugins.checkstyle.CheckStylePublisher', checkstyle: '**/target/checkstyle-result.xml'])  },
        "PMD" : { step([$class: 'hudson.plugins.pmd.PmdPublisher', checkstyle: '**/target/pmd.xml']) }
      )
   }
}