pipeline {
    environment {
      //PATH = "$PATH:bin"
      scannerHome = tool 'sonarscanner4.5'
      //sonarscanner = '${scannerHome}/SonarScanner.MSBuild'
    }
    agent any

    stages {
      stage('Build'){
        steps {
          //echo '${sonarscanner}'
          // This has to match the name you gave the sonarqube server on jenkins configuration
          withSonarQubeEnv('sonarqube-jenkins-local') {
            // SonarScanner.MSBuild.dll is being called by this python script
            sh 'python3 bin/build.py -k jenkins-dotnet --sonar-begin --build --test --coverlet --sonar-end --sonar-scanner "${scannerHome}/SonarScanner.MSBuild" \
                                     -d sonar.cs.opencover.reportsPaths=calculation.opencover.xml,prime.opencover.xml \
                                     -d sonar.dotnet.visualstudio.solution.file=unit-testing-using-dotnet-test.sln'
          }
        }
      }
      stage('Dotnet Test') {
        steps {
          sh 'python3 bin/build.py --test'
        }
      }
      stage('Coverlet Code Coverage') {
        steps {
          sh 'python3 bin/build.py --coverlet'// --threshold 80'
        }
      }
      stage('Report Generator') {
        steps {
          sh 'python3 bin/build.py --report-generator'
        }
      }
      stage('SonarQube Quality Gate') {
        steps {
          //sh 'sleep 5s'
          // Just in case something goes wrong, pipeline will be killed after a timeout
          timeout(time: 2, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
          }
        }
      }
    }
}
