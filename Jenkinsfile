node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build app image') {
	sh "docker build --no-cache -t dotnetapp -f Dockerfile ."
    }
    stage('Build test image') {
	sh "docker build --pull --target testrunner -t dotnetapp:test -f Dockerfile ."
    }
	
    stage('Run Test image') {
	sh """
	docker run --name sample -d dotnetapp:test
	docker cp sample:/app/tests/TestResults .
	docker rm -f sample
	"""
    }

}   
