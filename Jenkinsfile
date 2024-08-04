
def jobName = env.JOB_NAME

pipeline{
    agent any
    tools {
        maven 'Maven'
    }

    parameters {
        string(name: 'branch', defaultValue: 'master', description: 'github branch')
    }

    /* triggers {
        pollSCM('* * * * *')
    } */

    triggers {
        GenericTrigger(
         genericVariables: [
          [key: 'action', value: '$.action'],
          [key: 'FEATURE_BRANCH', value: '$.pull_request.head.ref'],
          [key: 'CURRENT_BRANCH', value: '$.pull_request.base.ref']
         ],

         causeString: 'Triggered on $action',

         token: 'abc123',
         tokenCredentialId: '',

         printContributedVariables: true,
         printPostContent: true,

         silentResponse: false,
         shouldNotFlatten: false,

         regexpFilterText: '${FEATURE_BRANCH}'.split("/")[-1],

         regexpFilterExpression: jobName

        )
    }

    stages {
        stage('Init') {
            steps {
                timeout(time: 60, unit: 'SECONDS'){
                    input message: 'approve to build job'
                }
                echo "jobName: ${jobName}"
                echo "FEATURE_BRANCH: ${FEATURE_BRANCH}"
                echo "CURRENT_BRANCH: ${CURRENT_BRANCH}"
                echo "action: ${action}"
            }
        }
        stage('Build'){
            steps {

                sh 'mvn clean package'
                //sh "/usr/local/bin/docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }
    }
}
