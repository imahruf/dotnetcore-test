node {

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
	sh "docker build -t dotnetapp -f Dockerfile ."
    }

    stage('Test image') {
	sh "docker run --rm -v '\$(pwd)'/TestResults:/app/tests/TestResults dotnetapp:test"
        sh "docker stop dotnetapp:test"
        sh "docker rm dotnetapp:test"
    }

}
