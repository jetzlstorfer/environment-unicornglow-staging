pipeline {
  options {
    disableConcurrentBuilds()
  }
  agent {
    label "jenkins-maven"
  }
  environment {
    DEPLOY_NAMESPACE = "jx-staging"
  }
  stages {
    stage('Validate Environment') {
      steps {
        scm checkout
        docker.withRegistry('https://registry.hub.docker.com/', 'juergen-docker') {

            def customImage = docker.build("dynatracesockshop/front-end:latest")

            /* Push the container to the custom Registry */
            customImage.push()
        }
        sh 'kubectl cluster-info'

        
        /*
        container('maven') {
          


          dir('env') {
            //sh 'jx step helm build'
            sh 'kubectl cluster-info'
          }
        }
        */
      }
    }
    stage('Update Environment') {
      when {
        branch 'master'
      }
      steps {
        container('maven') {
          dir('env') {
            sh 'jx step helm apply'
          }
        }
      }
    }
  }
}
