pipeline {
    agent {
      kubernetes {
        defaultContainer 'maven-11'
        yamlFile 'KubernetesPodJava.yaml'
      }
    }

    stages {
        stage('package') {
            steps {
                sh "mvn package"
            }
        }
        stage("Push to Registry") {
          steps {
            container('kaniko') {
              echo "${env.GIT_COMMIT}"
              script {
                gitCommit = "${env.GIT_COMMIT}"
              }
              echo "${gitCommit}"
              script {
                    def repositoryName = sh(returnStdout: true, script: "basename -s .git ${env.GIT_URL}").trim()
                    echo "Repository Name: ${repositoryName}"
                    sh "/kaniko/executor --dockerfile `pwd`/Dockerfile --context `pwd` --build-arg GIT_COMMIT=$gitCommit --build-arg ARTIFACT=target/spring-boot-hello-world-lolc.jar --label org.opencontainers.image.revision=$gitCommit --destination=sharedregistry23.azurecr.io/$repositoryName:dev"
              }
            }
        }
      }
    }
}
