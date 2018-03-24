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
		   // bat "mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent verify org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar -Dmaven.test.failure.ignore=true -Dsonar.jacoco.reportPaths=${env.WORKSPACE}/target/jacoco.exec"
		   
		 junit '*/target/test-results/*.xml'
		    //step( [ $class: 'JacocoPublisher' ] )
		 step([$class: 'JacocoPublisher', execPattern: 'target/jacoco.exec'])
		 jacoco( 
		   //   execPattern: 'target/*.exec',
		     // classPattern: 'target/classes',
		      //sourcePattern: 'src/main/java',
		      //exclusionPattern: 'src/test*'
		)
            }
           
        }
	 
        stage('SonarQube analysis') {
            steps {
                echo 'Sonar Scanner'
               //	def scannerHome = tool 'sonar-scanner-3.0.3'
			    withSonarQubeEnv('SonarQube Server') {
			    	bat 'C:/sonar-scanner-3.0.3/bin/sonar-scanner'
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
