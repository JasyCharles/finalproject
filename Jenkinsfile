node {
       stage('SCM') {
         checkout scm
       }

      stage ('Build')  {
          git 'https://github.com/JasyCharles/finalproject.git'
             withMaven(maven: 'mvn') {
                sh 'mvn clean install'
                sh 'mvn clean compile test'
             }
       }
    
    
    post {
        always {
            archiveArtifacts artifacts: "build/libs/**/*.jar", fingerprint: true
            junit "build/reports/**/*.xml"
        }
    }
    
    stage ('SonarQube Analysis') {
        def scannerHome = tool 'sonarqube';

        withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }
   

    stage('Results') {
        junit '**/target/surefire=reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
    }

    stage ('SonarQube Analysis') {
        withSonarQubeEnv() {
            sh "$scannerHome}/bin/sonar-scanner"
        }
     }
}
