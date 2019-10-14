node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build app image') {
	sh "docker build -t dotnetapp -f Dockerfile ."
    }
    stage('Build test image') {
	sh "docker build --pull --target testrunner -t dotnetapp:test -f Dockerfile ."
    }
	
    stage('Test image') {
	sh "docker run --rm -v \"\$(pwd)\"/TestResults:/app/tests/TestResults dotnetapp:test"
    }

}   
