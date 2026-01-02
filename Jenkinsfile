pipeline{
    agent any
    
     stages {
        stage('Get Code') {
            steps { 
                // Obtener código del repo
                git 'https://github.com/anieto-unir/helloworld.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Eyyy, esto es python. No hay que compilar nada!!!'
                echo WORKSPACE
                bat 'dir'
            }
        }

        stage('Tests') {
            parallel {
                stage ('Unit') {
                    steps {
                        catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
                            bat '''
                                set PYTHONPATH=%WORKSPACE%
                                C:\\Users\\denis\\AppData\\Local\\Programs\\Python\\Python314\\python.exe -m pytest --junitxml=result-unit.xml test\\unit
                                '''
            }
        }
    }
        stage('Service') {
            steps {
                bat '''
                    set FLASK_APP=app\\api.py
                    start /B flask run
                    start /B java -jar C:\\DevOps\\wiremock\\wiremock-jre8-standalone-2.28.0.jar --port 9090 --root-dir test\\wiremock
                    timeout /t 10
					set PYTHONPATH=%WORKSPACE%
                    C:\\Users\\denis\\AppData\\Local\\Programs\\Python\\Python314\\python.exe -m pytest --junitxml=result-rest.xml test\\rest
                    '''
                }
            }
        }
    }    
        stage('Results') {
            steps {
                junit 'result*.xml'
            }
        }
     }
 }