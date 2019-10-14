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
	
    stage('Test container') {
       containerID = sh (
            script: "docker run -d dotnetapp:test", 
        returnStdout: true
        ).trim()
        echo "Container ID is ==> ${containerID}"
        sh "docker cp ${containerID}:app/tests/TestResults \"\$(pwd)\"/TestResults"
        sh "docker stop ${containerID}"
        sh "docker rm ${containerID}"
    }

}   
