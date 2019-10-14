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
	sh "docker run --rm -iv${PWD}:/app/tests/TestResults/*.trx dotnetapp:test sh -s <<EOF chown $(id -u):$(id -g) *.trx cp -a *.trx /${PWD}"
    }

}   
