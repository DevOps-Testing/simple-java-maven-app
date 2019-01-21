pipeline {
    agent {
        docker {
            image 'maven:3.6-alpine'
            args '-v $HOME/.m2:/root/.m2:z -u root'
            reuseNode true
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }        
        stage('List effective Maven settings') {
			steps {
				sh 'mvn help:effective-settings'
			}
		}
		stage('Upload to Nexus Repo') {
            steps {                
                sh 'mvn clean deploy ' //-Dmaven.test.skip=true
            }
        }
    }
}
