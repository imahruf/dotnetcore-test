pipeline {
    agent any
 stages {
    stage('Clone repository') {
        checkout scm
    }
    stage('Build app image') {
	sh "docker build -t dotnetapp -f Dockerfile ."
    }
    stage('Build test image') {
	sh "docker build --pull --target testrunner -t dotnetapp:test -f Dockerfile ."
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
                success {
                    echo 'Compile stage successful'
                }
                failure {
                    echo 'Compile stage failed'
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
	
}
