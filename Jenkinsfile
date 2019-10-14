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
	sh "docker run -i --rm -v \${PWD}/TestResults:/app/tests/TestResults dotnetapp:test /bin/bash << COMMANDS cp -a /app/tests/TestResults/* \${PWD}/TestResults chown -R \$(id -u):\$(id -u) /app/tests/TestResults COMMANDS"
    }

}   
