pipeline {
 agent any
 stages {
  stage('Build App Image') {
   steps {
    sh "docker build -t mahruf/dotnetapp -f Dockerfile ."
   }
  }
  stage('Build Test Image') {
   steps {
    sh "docker build --pull --target testrunner -t mahruf/dotnetapp:test -f Dockerfile ."
   }
  }
  stage('Run Test Image') {
   steps {
    catchError {
     sh "docker run --name dummy mahruf/dotnetapp:test"
    }
   }
   post {
    always {
     echo 'Terminate Container'
     sh "rm -rf \"\$(pwd)\"/TestResults"
     sh "docker cp dummy:/app/tests/TestResults \"\$(pwd)\"/TestResults"
     sh "docker rm dummy"
     step([$class: 'MSTestPublisher', testResultsFile: "**/TestResults/UnitTests.trx", failOnError: true, keepLongStdio: true])
    }
   }
  }
  stage('Analyse Code') {
    environment {
        scannerHome = tool 'sonar'
    }
    steps {
        withSonarQubeEnv('sonarqube') {
         withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
            sh "${scannerHome}/bin/sonar-scanner -X -e -Dsonar.verbose=true -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=dotnetcore-test -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=dnc -Dsonar.sources=dotnetapp/Program.cs,utils/StringUtils.cs -Dsonar.tests=tests/StringUtilsTests.cs"
         }
        }
     sleep(10)
        timeout(time: 10, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
  }
  
  stage('Push image') {
   
   steps {
    withDockerRegistry([ credentialsId: "docker-hub-credentials", url: "https://registry.hub.docker.com" ]) {
      sh 'docker push mahruf/dotnetapp:latest'
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
