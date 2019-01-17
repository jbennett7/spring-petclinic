pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    mvn package -B -DskipTests=true
                '''
      }
      post {
        always {
          junit '**/target/surefire-reports/**/*.xml'

        }

      }
    }
    stage('Scan App - Build Container') {
      parallel {
        stage('IQ-BOM') {
          steps {
            nexusPolicyEvaluation(iqApplication: 'petclinic', iqStage: 'build', iqScanPatterns: [[scanPattern: '']])
          }
        }
        stage('Static Analysis') {
          steps {
            echo '...run SonarQube or other SAST tools here'
          }
        }
        stage('Build Container') {
          steps {
            echo '...need to learn the build process first'
          }
        }
      }
    }
    stage('Test Container') {
      steps {
        echo '...run container and test it'
      }
      post {
        success {
          echo '...the Test Scan Passed!'

        }

        failure {
          echo '...the Test  FAILED'
          error '...the Container Test FAILED'

        }

      }
    }
    stage('Scan Container') {
      steps {
        echo '...TODO scan container'
      }
      post {
        success {
          echo '...the IQ Scan PASSED'
          postGitHub(commitId, 'success', 'analysis', 'Nexus Lifecycle Container Analysis succeeded', "${policyEvaluationResult.applicationCompositionReportUrl}")

        }

        failure {
          echo '...the IQ Scan FAILED'
          postGitHub(commitId, 'failure', 'analysis', 'Nexus Lifecycle Containe Analysis failed', "${policyEvaluationResult.applicationCompositionReportUrl}")
          error '...the IQ Scan FAILED'

        }

      }
    }
    stage('Publish Container') {
      when {
        branch 'master'
      }
      steps {
        echo '...figure out container'
      }
    }
  }
  tools {
    jdk 'jdk8'
    maven 'M3'
  }
}
