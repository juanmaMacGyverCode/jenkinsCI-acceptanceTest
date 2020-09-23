pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage("Compile") {
            steps {
                bat "gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                bat "gradlew test"
            }
        }
        /*stage("Code coverage") {
            steps {
        	    sh "./gradlew jacocoTestReport"
        	 	publishHTML (target: [
         	        reportDir: 'build/reports/jacoco/test/html',
         			reportFiles: 'index.html',
         			reportName: 'JacocoReport'
         	    ])
         		sh "./gradlew jacocoTestCoverageVerification"
         	}
        }
        stage("Static code analysis") {
            steps {
                sh "./gradlew checkstyleMain"
                publishHTML (target: [
                	reportDir: 'build/reports/checkstyle/',
                	reportFiles: 'main.html',
                	reportName: 'Checkstyle Report'
                ])
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQubePruebas') {
                    sh './gradlew sonarqube'
                }
            }
        }*/
        stage ("Package") {
            steps {
        	    sh "./gradlew build"
        	}
        }
        stage ("Probar si funciona Docker") {
            steps {
                sh "docker version"
            }
        }
        /*stage ("Docker build") {
            steps {
                sh "docker build -t juanmamacgyvercode/calculator ."
            }
        }
        stage ("Docker login") {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub',
                                   usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    sh "docker login --username $USERNAME --password $PASSWORD"
                }
            }
        }
        stage ("Docker push") {
            steps {
                sh "docker push juanmamacgyvercode/calculator"
            }
        }
        stage ("Deploy to staging") {
            steps {
                sh "docker run -d --rm -p 8765:8080 --name calculatorStaging juanmamacgyvercode/calculator"
            }
        }*/
        stage ("Prueba") {
                    steps {
                        sleep 60
                        sh "curl 'localhost:8765/sum?a=1&b=2'"
                    }
                }
        stage ("Acceptance test") {
            steps {
                sleep 60
                sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
            }
            /*post {
                always {
                    sh "docker stop calculatorStaging"
                }
            }*/
        }
    }
}
