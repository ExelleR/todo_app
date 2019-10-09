node {
    def newApp
    def registry = 'https://registry-1.docker.io/v2/'
	def imagename = "exeller/hello"
    def registryCredential = 'dockerhub'
	environment {
        PROJECT_ID = 'studies-252508'
        CLUSTER_NAME = 'jenkins-cluster'
        LOCATION = 'us-central1-a'
        CREDENTIALS_ID = 'multibranch'
    }
	stage('Git') {
		git 'https://github.com/ExelleR/todo_app.git'
	}
	stage('Build') {
		sh 'npm install'
	}
	stage('Test') {
		sh 'npm test'
	}
	stage('Building image') {
        docker.withRegistry( registry, registryCredential ) {
		    def buildName = imagename + ":'$BRANCH_NAME'_'$BUILD_NUMBER'"
			newApp = docker.build(buildName)
			newApp.push();
		        newApp.push('latest')
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
