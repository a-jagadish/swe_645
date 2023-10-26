pipeline {
    agent any
    environment {
        PROJECT_ID = 'rancher'
        CLUSTER_NAME = 'rancher'
        LOCATION = 'us-east-1a'
        DOCKERHUB_PASS = 'AJ@gmu-2024'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage('BuildWAR') {
            steps {
                echo 'Creating the Jar ...'
                sh 'java -version'
                sh 'rm -rf *.war'
                sh 'jar -cvf swe.war -C src/main/webapps .'
                sh 'docker login -u ajagadis -p ${DOCKERHUB_PASS}'
				sh 'docker build -t ajagadis/swe .'
            }
        }
		stage("Pushing image to docker"){
			steps{
				script{
					sh 'docker push ajagadis/swe'
				}
			}
		}
    
        stage("UpdateDeployment") {
            steps {
					sh 'kubectl rollout restart deploy cluster'
	    }
        }
    }
}
