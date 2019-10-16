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
	
    stage('Run Test image') {
	sh "docker run --name dummy dotnetapp:test"
	sh "rm -rf \"\$(pwd)\"/TestResults"
	sh "docker cp dummy:/app/tests/TestResults \"\$(pwd)\"/TestResults"
	sh "docker rm dummy"
	    
    }

}   
