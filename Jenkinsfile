pipeline {
    agent {
        kubernetes {
            yaml """
kind: Pod
metadata:
  name: default
spec:
  containers:
  - name: jnlp
    image: gudari/jenkins-agent:4.9-ranger-arm64
    imagePullPolicy: Always
"""
        }
    }
    environment {
        GITHUB_ORGANIZATION = 'gudari'
        GITHUB_REPO         = 'oozie'
        VERSION             = '5.2.1'
        GITHUB_TOKEN        = credentials('github_token')
    }
    stages {
        stage('Build') {
            steps {
                sh ("bin/mkdistro.sh -Puber -Ptez -Pspark-2 -DskipTests -Dhadoop.version=2.10.1")
                sh ("./create_release.sh $GITHUB_ORGANIZATION $GITHUB_REPO $VERSION $GITHUB_TOKEN")
            }
        }
    }
}
