pipeline {
	script{
    def newApp
    def registry = 'https://registry-1.docker.io/v2/'
	def imagename = "exeller/hello"
    def registryCredential = 'dockerhub'
	}
	agent any
	environment {
        PROJECT_ID = 'studies-252508'
        CLUSTER_NAME = 'jenkins-cluster'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'multibranch'
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
			sh 'npm test'
		}
		stage('Building image') {
			steps {
				docker.withRegistry( registry, registryCredential ) {
					def buildName = imagename + ":'$BRANCH_NAME'_'$BUILD_NUMBER'"
					newApp = docker.build(buildName)
					newApp.push();
						newApp.push('latest')
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
