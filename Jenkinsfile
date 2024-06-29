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
