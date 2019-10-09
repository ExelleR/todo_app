pipeline {
	
	agent any
	environment {
        PROJECT_ID = 'studies-252508'
        CLUSTER_NAME = 'my-cluster'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'Studies'
    }
	stages{
		stage('Git') {
			steps {
				
					git 'https://github.com/ExelleR/todo_app.git'
				
			}
		}
		stage('Build') {
			steps {
				
					sh 'npm install'
				
			}
		}
		stage('Test') {
			steps {
				
					sh 'npm test'
				
			}
		}
		stage('Building image') {
			steps {
				script {
					docker.withRegistry( 'https://registry-1.docker.io/v2/', 'dockerhub' ) {
						def buildName = "exeller/hello" + ":'$BRANCH_NAME'_'$BUILD_NUMBER'"
						def newApp = docker.build(buildName)
						newApp.push();
							newApp.push('latest')
					}
				}
			}
		}
		stage('Deploy to GKE') {
				steps{
					step([
					$class: 'KubernetesEngineBuilder',
					projectId: env.PROJECT_ID,
					clusterName: env.CLUSTER_NAME,
					location: env.LOCATION,
					manifestPattern: 'manifest.yaml',
					credentialsId: env.CREDENTIALS_ID,
					verifyDeployments: true])
				}
		}
	}	
}
