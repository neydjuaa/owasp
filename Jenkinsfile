pipeline {
    agent any

    tools {
        maven "MAVEN"
    }

    stages {
        stage('Build & Unit test'){
            steps {
                // Run maven clean and package
                sh 'sudo mvn clean package'

                // Publish JUnit test results
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }

        stage('Security SAT : OWASP Dependency Check'){
            steps {
                //Run AWASP Dependency Check
                sh "sudo mvn verify"
            }
        }

        stage('Deploy to tomcat'){
            steps{
                script {
                    // Deploy to tomcat
                    sh 'copy target/MyWebAppSecurity.war /opt/tomcat/webapps/mywebapp.war'
                }
            }
        }

        stage('Scan ZAP(Active Scan DAST)'){
            steps {
                script {
                    // Run ZAP Docker container for Active Scan elevated privileges

                    sh 'docker run --rm -v /home/vagrant:/zap/wrk/:rw -t ghcr.io/zaproxy:stable zap-baseline.py -t http://13.38.98.152:900/mywebapp/ -x report_OWASP || exit 0'
                }
            }
        }
    }
}