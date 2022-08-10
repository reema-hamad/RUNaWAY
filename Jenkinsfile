pipeline {

	agent any

	environment {
		DOCKERHUB_CREDENTIALS=credentials('Reema-dockerhub-token')
		AWS_ACCESS_KEY_ID     = credentials('Reema-aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('Reema-aws-secret-access-key')
		ARTIFACT_NAME = 'Dockerrun.aws.json'
		AWS_S3_BUCKET = 'Reemabelt2artifacts1234561'
		AWS_EB_APP_NAME = 'Reema1-Belt2-EB1-123456'
        AWS_EB_ENVIRONMENT_NAME = 'Reemabelt2artifacts1234561-env'
        AWS_EB_APP_VERSION = "${BUILD_ID}"
	}

	stages {

		stage('Build') {

			steps {
				sh 'docker build -t rmrmhm23/runway:latest .'
			}
		}

		stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push') {

			steps {
				sh 'docker push rmrmhm23/runway:latest '
			}
		}

        stage('Deploy') {
            steps {
                sh 'aws configure set region us-east-1	'
                sh 'aws elasticbeanstalk create-application-version --application-name $AWS_EB_APP_NAME --version-label $AWS_EB_APP_VERSION --source-bundle S3Bucket=$AWS_S3_BUCKET,S3Key=$ARTIFACT_NAME'
                sh 'aws elasticbeanstalk update-environment --application-name $AWS_EB_APP_NAME --environment-name $AWS_EB_ENVIRONMENT_NAME --version-label $AWS_EB_APP_VERSION'
            }
	}
    }
	post {
		always {
			sh 'docker logout'
		}
	}

}