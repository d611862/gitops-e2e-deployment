pipeline {
	agent {
		label "jenkins-agent"
	}
	environment {
		APP_NAME = "cicd-production-pipeline"
	}
	stages {
		stage("Cleanup workspace") {
			steps {
				cleanWs()
			}
		}

		stage("Checkout form SCM") {
			steps {
				git branch: 'main', credentialsId: 'github', url: 'https://github.com/d611862/gitops-e2e-deployment'
			}
		}

		stage("Update deployment tag") {
			steps {
				sh """
					cat deployment.yaml
					sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
					cat deployment.yaml
				"""
			}
		}

		stage("push changed deployment file to git") {
			steps {
				sh """
					git config --global user.name "d611862"
					git config --global user.email "shehan.perera@gmail.com"
					git add deployment.yaml
					git commit -m "updated deployment manifest with new image tag"
				"""
				withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
					sh "git push https://github.com/d611862/gitops-e2e-deployment main"
				}
			}
		}
	}
}
