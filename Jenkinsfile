pipeline {
    agent any

    environment {
        M2_INSTALL = "/usr/bin/mvn"
    }

    stages {
        stage('Clone-Repo') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=false'
            }
        }
        
        stage('Unit Tests') {
            steps {
                sh 'mvn compiler:testCompile'
                sh 'mvn surefire:test'
                junit 'target/**/*.xml'
            }
        }

        stage('Deployment') {
            steps {
                sh 'sshpass -p "Japan@99" scp target/gamutkart.war manish99@172.17.0.2:/home/manish99/apache-tomcat-9.0.90/webapps'
                sh 'sshpass -p "Japan@99" ssh manish99@172.17.0.2 "/home/manish99/apache-tomcat-9.0.90/bin/startup.sh"'
            }
        }

        stage('Send Email Notification') {
            steps {
                script {
                    def subject = "Build ${currentBuild.result}: ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
                    def body = """
                        <p>Job: ${env.JOB_NAME}</p>
                        <p>Build Number: ${env.BUILD_NUMBER}</p>
                        <p>Result: ${currentBuild.result}</p>
                        <p><a href="${env.BUILD_URL}">Click here to view the build</a></p>
                    """
                    
                    emailext (
                        to: 'neetuy90@gmail.com', // Update with recipient's email address
                        subject: subject,
                        body: body,
                        mimeType: 'text/html'
                    )
                }
            }
        }
    }
}

