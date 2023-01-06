node {
	def application = "webapp"
	def dockerhubaccountid = "ppatel253"
	stage('Clone repository') {
		checkout scm
	}

	stage('Build image') {
		app = docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
	}

	stage('Push image') {
		withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
		app.push()
		app.push("latest")
	}
	}

	stage('Deploy') {
		steps {
			ansiblePlaybook credentialsId: 'ansiblekey', inventory: '/etc/ansible/hosts', playbook: 'phpplaybook.yml'
		}
	}
}
