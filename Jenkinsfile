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
                sh label: '', script: 'mvn clean compile'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh label: '', script: 'mvn test'
                
            }
        }
        stage('JaCoCo') {
            steps {
                echo 'Code Coverage'
		jacoco buildOverBuild: true, changeBuildStatus: true
               
            }
        }
        stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
               	//def scannerHome = tool 'SonarQube Scanner 3.0'
			    withSonarQubeEnv('sonarserver') {
			    	sh label: '', script: 'mvn clean package sonar:sonar'
			    }
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging'
		sh label: '', script: 'mvn package -DskipTests'
                
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
