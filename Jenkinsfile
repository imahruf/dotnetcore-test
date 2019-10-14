node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build app image') {
	sh "docker build --no-cache -t dotnetapp -f Dockerfile ."
    }
    stage('Build test image') {
	sh "docker build --pull --target testrunner -v \"\$(pwd)\"/TestResults:/app/tests/TestResults -t dotnetapp:test -f Dockerfile ."
    }
	


}   
