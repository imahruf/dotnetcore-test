pipeline {
    agent any
 stages {
    stage('Build app image') {
	    steps {
	sh "docker build -t dotnetapp -f Dockerfile ."
	    }
    }
    stage('Build test image') {
	    steps {
	sh "docker build --pull --target testrunner -t dotnetapp:test -f Dockerfile ."
	    }
    }
    stage('Run Test image') {
	    steps {
	    	catchError {
			sh "docker run --name dummy dotnetapp:test"   
			
	    	}
    }
    stage('sonar-scanner') {
      def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
      withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
        sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.verbose=true -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=dotnetcore-test -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=dot -Dsonar.sources=**/dotnetapp/*.cs -Dsonar.sources=**/utils/*.cs -Dsonar.tests=**/tests/*.cs -Dsonar.exclusions=*.json"
      }
    }
	    
    post {
                always {
                    echo 'Terminate Container'
			sh "rm -rf \"\$(pwd)\"/TestResults"
			sh "docker cp dummy:/app/tests/TestResults \"\$(pwd)\"/TestResults"
			sh "docker rm dummy"
			step ([$class: 'MSTestPublisher', testResultsFile:"**/TestResults/UnitTests.trx", failOnError: true, keepLongStdio: true])
                }
            }
    }
}   
post {
        success {
            echo 'whole pipeline successful'
        }
        failure {
            echo 'pipeline failed, at least one step failed'
        }
    }	
}
