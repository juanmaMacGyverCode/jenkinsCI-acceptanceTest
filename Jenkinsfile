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
        	    bat "gradlew build"
        	}
        }
        stage ("Probar si funciona Docker") {
            steps {
                bat "docker version"
            }
        }
        stage ("Docker build") {
            steps {
                bat "docker build -t juanmamacgyvercode/calculator ."
            }
        }
        stage ("Docker login") {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub',
                                   usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    bat "docker login --username $USERNAME --password $PASSWORD"
                }
            }
        }
        stage ("Docker push") {
            steps {
                bat "docker push juanmamacgyvercode/calculator"
            }
        }
        stage ("Deploy to staging") {
            steps {
                bat "docker run -d --rm -p 8765:8080 --name calculatorStaging juanmamacgyvercode/calculator"
            }
        }
        stage ("Prueba") {
                    steps {
                        sleep 60
                        bat "curl 'localhost:8765/sum?a=1&b=2'"
                    }
                }
        /*stage ("Acceptance test") {
            steps {
                sleep 60
                sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
            }*/
            /*post {
                always {
                    sh "docker stop calculatorStaging"
                }
            }*/
        /*}*/
    }
}
