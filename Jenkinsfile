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
	sh "docker create -ti --name dummy dotnetapp:test bash" 
	sh "docker cp dummy:/app/tests/TestResults \"\$(pwd)\"/TestResults"
	sh "docker rm -f dummy"
    }

}   
