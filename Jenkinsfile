pipeline {
  agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Checkout'
            }
        }
        stage('Build') {
            steps {
                echo 'Clean Build'
                bat 'mvn clean compile'
            }
        }
       
	stage ('Functional tests') {
            steps {
                bat "mvn verify"
		    bat "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent verify org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar -Dmaven.test.failure.ignore=true -Dsonar.jacoco.reportPaths=${env.WORKSPACE}/target/jacoco.exec"

            }
           
        }
	 
        stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
               	//def scannerHome = tool 'SonarQube Scanner 3.0'
			    withSonarQubeEnv('SonarQube Server') {
			    	bat 'SonarQubeScanner-2.4'
			    }
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging'
                bat 'mvn package -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                echo '## TODO DEPLOYMENT ##'
            }
        }
    }
    
    post {
        always {
            echo 'JENKINS PIPELINE'
        }
        success {
            echo 'JENKINS PIPELINE SUCCESSFUL'
        }
        failure {
            echo 'JENKINS PIPELINE FAILED'
        }
        unstable {
            echo 'JENKINS PIPELINE WAS MARKED AS UNSTABLE'
        }
        changed {
            echo 'JENKINS PIPELINE STATUS HAS CHANGED SINCE LAST EXECUTION'
        }
    }
}
