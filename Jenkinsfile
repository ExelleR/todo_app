node {
    def newApp
    def registry = 'https://registry-1.docker.io/v2/'
	def imagename = "exeller/hello"
    def registryCredential = 'dockerhub'
	
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
}
