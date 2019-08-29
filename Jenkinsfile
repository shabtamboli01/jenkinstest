node{
    stage('SCM Checkout'){
    git branch: 'patch-1', credentialsId: 'Git_Credential', url: 'https://github.com/shabtamboli01/hello-world.git'
                        }
    stage('mvn built task'){
        sh label: '', script: 'mvn clean install package'
    }
    stage('sonarqube Test'){
        withSonarQubeEnv(credentialsId: 'token1'){
           sh label: '', script: 'mvn clean package sonar:sonar'
        }        
    }
   stage('Publish Artificats To Nexus Repo'){
     nexusPublisher nexusInstanceId: 'nexus3', nexusRepositoryId: 'myrepo', packages: [[$class: 'MavenPackage', mavenAssetList: [[classifier: '', extension: '', filePath: '/var/lib/jenkins/workspace/Demo2_Project/webapp/target/webapp.war']], mavenCoordinate: [artifactId: 'spring3', groupId: 'group1', packaging: 'war', version: '2.0']]]
}
   stage('Copy Artifacts And Run Playbook'){
         sshPublisher(publishers: [sshPublisherDesc(configName: 'ansibleserver', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook  /opt/playbook/artifac.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
   }

  }

==============================================

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
                jacoco()
            }
        }
        stage('Sonar') {
            steps {
                echo 'Sonar Scanner'
               	//def scannerHome = tool 'SonarQube Scanner 3.0'
			    withSonarQubeEnv(credentialsId: 'token1') {
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
