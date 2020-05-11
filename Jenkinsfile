pipeline {
  agent any

	parameters {
		choice(name: 'TARGET_ENV', choices: ['staging', 'production', 'ec2'], description: 'Please choose an environment')
	}

  stages {
    stage('Copy artifacts') {
      steps {
        copyArtifacts filter: 'multygo_master', fingerprintArtifacts: true, projectName: 'multygo/master', selector: lastSuccessful()
      }
    }
	stage('Deliver to prod') {
            steps {
                ansiblePlaybook colorized: true,
                    
                    credentialsId: '4e9e04f8-0f41-4529-97c2-02f39c3bcb8a',
                    disableHostKeyChecking: true,
                    installation: 'asinble',
                    inventory: 'environments/production/host.ini',
                    playbook: 'playbook.yml'
            }
    }
	stage('tests prod') {
		steps {
			sh 'docker run -v $HOME/workspace/example1-deployment/environments/${params.TARGET_ENV}:/etc/newman -t postman/newman run "https://www.getpostman.com/collections/434a10daa020cc392009" -e prodoction.postman_environment.json'
                }
        }
  }
