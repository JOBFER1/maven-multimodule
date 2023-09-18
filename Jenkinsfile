pipeline{
    agent any
    triggers { 
    	githubPush() 
   	}      
    stages{
        stage ('Compilar') {
            steps {
                bat 'mvn clean verify'
            }
        }
        stage('Test y Jacoco') {
            steps {
                junit '**/*.xml'
                jacoco()
            }
        }
        stage('Sonar Scanner Coverage') {
            steps{
//                withSonarQubeEnv(installationName: 'Sonar 7.9.6', credentialsId: 'Token-Sonar') {
                withSonarQubeEnv(installationName: 'Sonar 10.2.1', credentialsId: 'Token-Sonar-10') {
                 bat 'mvn sonar:sonar'
                }
            }
        }
        stage ('Nexus') {
            steps {
                bat 'mvn clean deploy'
            }
        }
    }
    post {
        always {
            cleanWs()
            dir("${env.WORKSPACE}@tmp") {
                deleteDir()
            }
            dir("${env.WORKSPACE}@script") {
                deleteDir()
            } 
        }
    }
}