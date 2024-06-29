pipeline {
    agent any

//	tools {
//		jdk 'jdk8'
//	}
//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
        stage('Clone-Repo') {
	    	steps {
	        	checkout scm
	    	}
        }

        stage('Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
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
    }
}
post {
        success {
            emailext (
                to: 'mmalhotra419@gmail.com',
                subject: 'Build Success: ${env.JOB_NAME} [${env.BUILD_NUMBER}]',
                body: '''
                    <p>Good news!</p>
                    <p>The build was successful.</p>
                    <p>Job: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p><a href="${env.BUILD_URL}">Click here to view the build</a></p>
                ''',
                mimeType: 'text/html'
            )
        }

        failure {
            emailext (
                to: 'mmalhotra419@gmail.com',
                subject: 'Build Failed: ${env.JOB_NAME} [${env.BUILD_NUMBER}]',
                body: '''
                    <p>Unfortunately, the build failed.</p>
                    <p>Job: ${env.JOB_NAME}</p>
                    <p>Build Number: ${env.BUILD_NUMBER}</p>
                    <p><a href="${env.BUILD_URL}">Click here to view the build</a></p>
                ''',
                mimeType: 'text/html'
            )
        }
    }
