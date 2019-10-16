pipeline {
    agent any
 stages {
    stage('Clone repository') {
	    steps {
        checkout scm
	    }
    }
    stage('Build app image') {
	    steps {
	sh "docker build -t dotnetapp -f Dockerfile ."
	    }
    }
    stage('Build test image') {
	    steps {
	sh "docker build --pull --target testrunner -t dotnetapp:test -f Dockerfile ."
	    }
    }
    stage('Run Test image') {
	    steps {
	    	catchError {
			sh "docker run --name dummy dotnetapp:test"   
			
	    	}
	    }
	    post {
                always {
                    echo 'Terminate Container'
			sh "rm -rf \"\$(pwd)\"/TestResults"
			sh "docker cp dummy:/app/tests/TestResults \"\$(pwd)\"/TestResults"
			sh "docker rm dummy"
                }
            }
    }
}   
post {
        success {
            echo 'whole pipeline successful'
        }
        failure {
            echo 'pipeline failed, at least one step failed'
        }
    }	
}
