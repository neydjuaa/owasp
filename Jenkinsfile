pipeline {
    agent any

    tools {
        maven "maven3.9.6"
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

       
        stage('Deploy to tomcat'){
            steps{
                script {
                    // Deploy to tomcat
                    sh 'sudo cp target/MyWebAppSecurity.war /opt/tomcat/webapps/mywebapp.war'
                }
            }
        }

        stage('Scan ZAP(Active Scan DAST)'){
            steps {
                script {
                    // Run ZAP Docker container for Active Scan elevated privileges

                    sh 'docker run --rm -v /home/vagrant:/zap/wrk/:rw -t ghcr.io/zaproxy:stable zap-baseline.py -t http://192.168.56.5:8080/mywebapp/ -x report_OWASP || exit 0'
                }
            }
        }
    }
}