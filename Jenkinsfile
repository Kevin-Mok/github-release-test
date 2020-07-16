@Library('jenkins-pipeline-shared-libraries')_

pipeline {
    agent { label 'kie-rhel7-priority' }
    /* agent any */
    stages {
        stage('Release') {
            steps {
                script {
                    cleanWs()
                    def checkoutBranch = "7.14.3-SNAPSHOT"
                    checkout(githubscm.resolveRepository("kogito-examples", "Kevin-Mok-Bot", checkoutBranch, false))
                    sh """
                    git checkout ${checkoutBranch}
                    """
                    githubscm.createBranch("stable")
                    withCredentials([usernamePassword(credentialsId: "kmok-github-bot", usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh("git config --local credential.helper \"!f() { echo username=\\$GIT_USERNAME; echo password=\\$GIT_PASSWORD; }; f\"")
                        sh("git push --force origin stable")
                    }
                }
            }
        }
    }
}
