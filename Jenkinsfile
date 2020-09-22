pipeline {
    environment {
        registry = "juanmamacgyvercode/calculator"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                sh "./gradlew test"
            }
        }
        stage("Code coverage") {
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
        }
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
        stage ("Docker build") {
            steps {
                env.dockerImage = docker.build registry + ":$BUILD_NUMBER"
                /*sh "docker build -t juanmamacgyvercode/calculator ."*/
            }
        }
        /*stage ("Docker login") {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker-hub-credentials',
                                   usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    sh "docker login --username $USERNAME --password $PASSWORD"
            }
        }*/
        stage ("Docker push") {
            steps {
                docker.withRegistry('', registryCredential) {
                    dockerImage.push()
                }
                /*sh "docker push juanmamacgyvercode/calculator"*/
            }
        }
        stage('Cleaning up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
