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
			sh "rm -rf \"\$(pwd)\"/TestResults"
			sh "docker cp dummy:/app/tests/TestResults \"\$(pwd)\"/TestResults"
			sh "docker rm dummy"
	    	}
	    }
	    post {
                always {
                    echo 'Mahruf Compile stage successful'
                }
            }
    }
}   
	
}
