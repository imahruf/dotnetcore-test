node {
    def app
    def apptest

    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
	sh "docker build -t dotnetapp:B${BUILD_NUMBER} -f ./dotnetapp/Dockerfile.Build ."
        sh "docker build -t dotnetapp:test:B${BUILD_NUMBER} -f ./dotnetapp/Dockerfile.Test ."
        sh "docker build -t dotnetapp:run:B${BUILD_NUMBER} -f ./dotnetapp/Dockerfile.Run ."
    }

    stage('Test image') {
	sh "docker run --rm -v '$(pwd)'/TestResults:/app/tests/TestResults dotnetapp:test"
        sh "docker stop dotnetapp:test"
        sh "docker rm dotnetapp:test"
    }

}
