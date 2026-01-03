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

        stage('Service') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'SUCCESS') {
                    bat '''
                        set FLASK_APP=app\\api.py
                         start "" C:\\Users\\denis\\AppData\\Local\\Programs\\Python\\Python314\\python.exe app\\api.py
                        
                        ping -n 10 127.0.0.1

                        C:\\Users\\denis\\AppData\\Local\\Programs\\Python\\Python314\\python.exe -m pytest --junitxml=result-rest.xml test\\rest
                    '''
                }
				junit 'result-rest.xml'
            }
        }

    }
}
