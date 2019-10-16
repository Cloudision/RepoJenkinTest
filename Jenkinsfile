#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    
    def HUB_ORG="aasif.sfdc@gmail.com"
    def SFDC_HOST="https://asiftech-dev-ed.my.salesforce.com"
    def CONNECTED_APP_CONSUMER_KEY="3MVG9Y6d_Btp4xp5jXRtDdzWrG0fMO9_ITSbNJE7MQoiYm1MV_N3OxMDvKBDJOCclIGX_MaMIsBjICCM2nlOG"
    def JWT_KEY_CRED_ID="e7f812f5-36e5-4c5f-801c-c283f19cf01c"

    def toolbelt = tool 'sfdx'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        stage('Create Scratch Org') {

            rc = sh returnStatus: true, script: "${toolbelt}/sfdx force:auth:jwt:grant -u ${HUB_ORG} -f \"${jwt_key_file}\" -i ${CONNECTED_APP_CONSUMER_KEY} -r ${SFDC_HOST}"
            if (rc != 0) { error 'hub org authorization failed' }

            // need to pull out assigned username
          /*  rmsg = sh returnStdout: true, script: "${toolbelt}/sfdx force:org:create --definitionfile project-scratch-def.json --json --setdefaultusername"
            printf rmsg
            def jsonSlurper = new JsonSlurperClassic()
            def robj = jsonSlurper.parseText(rmsg)
            if (robj.status != 0) { error 'org creation failed: ' + robj.message }
            SFDC_USERNAME=robj.result.username
            robj = null*/

        }

}
