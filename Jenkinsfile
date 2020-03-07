node {

    def dockerImage
    def registryCredential = 'DockerHub'
    def githubCredential = 'Github'
    def commit_id
	
	stage('Clone repository') {
        /* Cloning the Repository to our Workspace */



        sh 'rm helmChart -rf; mkdir helmChart'
        dir('webapp-backend') {
                git branch: 'Testing',
                credentialsId: githubCredential,
                git url: 'https://github.com/Jagman13/webapp-backend.git'
            }

    }

    stage('Clone another repository') {
            /* Cloning the Repository to our Workspace */



            sh 'rm helmChart -rf; mkdir helmChart'
                dir('helmChart') {
                    git branch: 'Testing-Jenkins',
                    credentialsId: githubCredential,
                    git url: 'https://github.com/Jagman13/helm-charts.git'
             }
        }
	stage('Building image') {
        dir('webapp-backend'){
        commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD')
  		echo "$commit_id"
        dockerImage = docker.build ("${env.registry}")
        }
	}
	stage('Registring image') {
	    dir('webapp-backend'){
        docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$commit_id")
		}
	 }
    }

    stage('updating helm repo'){
     dir('helmChart'){
    // sh 'git merge develop'
     sh('mkdir testing')
     sh 'git commit -m "Merged develop branch to master'
     sh "git push origin Testing-Jenkins"
     }

    }
}