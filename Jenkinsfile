pipeline {
	agent any
	tools {
		go {'go'}
	}
	environment {
		XDG_CACHE_HOME = '/tmp/.cache'
		CGO_ENABLED='0'
	}
	
	stages {
		stage('Get dependencies') { 
			steps {
				sh "go get \"github.com/aws/aws-lambda-go/lambda\""
			}
		}
		stage('Build') { 
			steps {
				sh 'GOOS=linux GOARCH=amd64 go build -o golambda golambda.go'
				sh 'zip golambda.zip golambda'
			}
		}
		stage('Publish') { 
			steps { 
				archiveArtifacts 'golambda.zip'
			}
		}
	}
}
