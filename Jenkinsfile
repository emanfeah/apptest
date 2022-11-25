pipeline {

	agent any

	environment {
		DOCKERHUB_CREDENTIALS = credentials('eman-dockerhub')
		AWS_ACCESS_KEY_ID     = credentials('AWS_Access_KeyId')
  		AWS_SECRET_ACCESS_KEY = credentials('AWS_Secret_Key')

	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t emanfeah/runway:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push emanfeah/runway:latest'
			}
		}
		stage('Deploying App to Kubernetes') {
         steps {
           script {
            kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
           }
         }
        }

      
        stage ("Logout") {
            steps {
                sh 'docker logout'
            }
    }



    }
	 

}