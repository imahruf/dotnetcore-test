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
	sh "docker run --name dummy -v \"\$(pwd)\"/TestResults/*.trx:/app/tests/TestResults/*.trx dotnetapp:test"
	sh "docker cp dummy:/app/tests/TestResults/TResults.trx \"\$(pwd)\"/TestResults/."
	sh "docker rm dummy"
	    
    }

}   
