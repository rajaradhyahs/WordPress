node {

	def application = "wordpress"
	def dockerhubaccountid = "rajaradhyahs"
	stage('Clone repository') {
		checkout scm
	}


	stage('Build image') {
		app = docker-compose up -d ("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}


	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
		app.push()
		app.push("latest")
	}

	}
	stage('Deploy') {
		sh ("docker run -d -p 81:8080 -v /var/log/:/var/log/ ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}
	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }

}
