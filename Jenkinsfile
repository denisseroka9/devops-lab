pipeline {
    agent any

    stages {

        stage('Get Code') {
            steps {
                // Obtener código del repo
                git 'https://github.com/anieto-unir/helloworld.git'
            }
        }

		stage('Unit') {
			steps {
					bat '''
						set PYTHONPATH=.
						pytest --junitxml=result-unit.xml test\\unit
					'''
					
					junit 'result-unit.xml'
				}
			}
		}

		stage('Service') {
			steps {
				catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
					bat '''
						set FLASK_APP=app\\api.py
						start flask run
						start java -jar C:\\UNIR\\Ejercicios\\wiremock-jre8-standalone-2.28.0.jar --port 9090 --root-dir test\\wiremock

						ping -n 10 127.0.0.1

						pytest --junitxml=result-rest.xml test\\rest
					'''
				}
			}
		}

    }
}
