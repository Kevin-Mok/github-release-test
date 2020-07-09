deployProperties = [:]

pipeline {
    agent any
    environment {
        PROPERTIES_FILE_NAME = "deployment.properties"
    }
    parameters {
        booleanParam(name: 'RELEASE', defaultValue: false, description: 'Is this build for a release?')
        string(name: 'DEPLOY_BUILD_URL', defaultValue: 'http://localhost:8090/job/2287/7/', description: 'URL to jenkins deploy build to retrieve the `deployment.properties` file. If base parameters are defined, they will override the `deployment.properties` information')
    }
    stages {
        stage('Approve promotion') {
            when {
                expression { return params.RELEASE && params.DEPLOY_BUILD_URL != '' }
            }
            steps {
                script {
                    readDeployProperties()
                    echo deployProperties.collect{ entry ->  "${entry.key}=${entry.value}" }.join("\n")
                    echo isRelease()
                }
            }
        }
    }
}

//////////////////////////////////////////////////////////////////////////////
// Deployment properties
//////////////////////////////////////////////////////////////////////////////

void readDeployProperties(){
    if (params.DEPLOY_BUILD_URL != null){
        withCredentials([usernamePassword(credentialsId: 'kmok-jenkins', usernameVariable: 'JENKINS_USER', passwordVariable: 'JENKINS_TOKEN')]) {
            sh "wget --auth-no-challenge --user=${JENKINS_USER} --password=${JENKINS_TOKEN} ${params.DEPLOY_BUILD_URL}artifact/${PROPERTIES_FILE_NAME}"
        }
        /* sh "cat ${PROPERTIES_FILE_NAME}" */
        deployProperties = readProperties file: PROPERTIES_FILE_NAME
    }
}

boolean hasDeployProperty(String key){
    return deployProperties[key] != null
}

String getDeployProperty(String key){
    if(hasDeployProperty(key)){
        return deployProperties[key]
    }
    return ""
}

String getParamOrDeployProperty(String paramKey, String deployPropertyKey){
    if (params[paramKey] != ""){
        return params[paramKey]
    }
    return getDeployProperty(deployPropertyKey)
}

//////////////////////////////////////////////////////////////////////////////
// Getter / Setter
//////////////////////////////////////////////////////////////////////////////

boolean isRelease() {
    return params.RELEASE || (getDeployProperty("release") == "true")
}

String getProjectVersion() {
    return getParamOrDeployProperty("PROJECT_VERSION", "project.version")
}

String getGitTag() {
    return params.GIT_TAG != "" ? params.GIT_TAG : getProjectVersion()
}

String getBuildBranch() {
    return getParamOrDeployProperty("BUILD_BRANCH_NAME", "git.branch")
}
