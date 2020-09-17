pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage("Compile") {
            steps {
                if (isUnix()) {
                    sh "gradlew compileJava"
                } else {
                    bat "gradlew compileJava"
                }
            }
        }
        stage("Unit test") {
            steps {
                sh "gradlew test"
            }
        }
        stage("Code coverage") {
            steps {
        	    sh "gradlew jacocoTestReport"
        	 	publishHTML (target: [
         	        reportDir: 'build/reports/jacoco/test/html',
         			reportFiles: 'index.html',
         			reportName: 'JacocoReport'
         	    ])
         		sh "gradlew jacocoTestCoverageVerification"
         	}
        }
        stage("Static code analysis") {
            steps {
                sh "gradlew checkstyleMain"
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
                    sh 'gradlew sonarqube'
                }
            }
        }
        stage ("Package") {
        	 	steps {
        	 		sh "gradlew build"
        	 	}
        }
        stage ("Probar si funciona Docker") {
                	 	steps {
                	 		sh "docker version"
                	 	}
                }
        stage ("Docker build") {
        	 	steps {
        	 		sh "docker build --privileged -t juanmamacgyvercode/calculator ."
        	 	}
        }
    }
}
