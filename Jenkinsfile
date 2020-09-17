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
        stage("Code coverage") {
            steps {
        	    bat "gradlew jacocoTestReport"
        	 	publishHTML (target: [
         	        reportDir: 'build/reports/jacoco/test/html',
         			reportFiles: 'index.html',
         			reportName: 'JacocoReport'
         	    ])
         		bat "gradlew jacocoTestCoverageVerification"
         	}
        }
        stage("Static code analysis") {
            steps {
                bat "gradlew checkstyleMain"
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
                    bat 'gradlew sonarqube'
                }
            }
        }
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
    }
}
