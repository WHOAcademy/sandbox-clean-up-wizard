pipeline {
    agent {
        label "master"
    }
    environment {
        NAMESPACE = 'labs-dev'
    }
    parameters {
        string(name: 'NAMESPACE', defaultValue: 'labs-dev', description: 'The namepsace to clean up all objects from')
    }
    // The options directive is for configuration that applies to the whole job.
    options {
        buildDiscarder(logRotator(numToKeepStr: '50', artifactNumToKeepStr: '2'))
        ansiColor('xterm')
    }
    triggers { 
        cron('@daily, @midnight') 
    }
    stages {
        stage("system tests") {
            agent {
                node {
                    label "master"
                }
            }
            steps {
                echo '### 🙀🗡 Kill -9 project resources [svc, route, dc, pvc] ###'
                sh '''
                    oc project ${NAMESPACE}
                    oc get dc --no-headers -n ${NAMESPACE} | cut -d' ' -f 1 | xargs oc delete -n ${NAMESPACE} dc
                    oc get svc --no-headers -n ${NAMESPACE} | cut -d' ' -f 1 | xargs oc delete -n ${NAMESPACE} svc
                    oc get route --no-headers -n ${NAMESPACE} | cut -d' ' -f 1 | xargs oc delete -n ${NAMESPACE} route
                    oc get pvc --no-headers -n ${NAMESPACE} | cut -d' ' -f 1 | xargs oc delete -n ${NAMESPACE} pvc
                '''
            }
        }
    }
}
