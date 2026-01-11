pipeline {
    agent any

    stages {

        stage('Get Code') {
            steps {
                git 'https://github.com/anieto-unir/helloworld.git'
            }
        }

        stage('Unit') {
            steps {
                bat '''
                    set PYTHONPATH=.
                    C:\\Users\\denis\\AppData\\Local\\Programs\\Python\\Python314\\python.exe -m pytest --junitxml=result-unit.xml test\\unit 
                '''
                junit 'result-unit.xml'
            }
        }
		
		stage('Static') {
			steps {
				bat '''
					C:\\Users\\denis\\AppData\\Local\\Programs\\Python\\Python314\\python.exe -m flake8 --exit-zero app --format=pylint > flake8.out
				'''
				recordIssues tools: [flake8(name: 'Flake8', pattern: 'flake8.out')], qualityGates: [[threshold: 8, type: 'TOTAL', unstable: true],[threshold: 10, type: 'TOTAL', unstable: false]]				
			}
		}

		stage('Security') {
			steps {
				catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
					bat '''
						bandit --exit-zero -r app -f custom -o bandit.out --msg-template "{abspath}:{line}: [{test_id}] {msg}"
					'''
					recordIssues(
						tools: [pylint(name: 'Bandit', pattern: 'bandit.out')],
						qualityGates: [
							[threshold: 1, type: 'TOTAL', unstable: true],
							[threshold: 2, type: 'TOTAL', unstable: false]
						]
					)
				}
			}
		}

		
    }
}
