#!groovy
import groovy.json.JsonSlurperClassic
node {


    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME

    def  HUB_ORG_DH="aasif.sfdc@gmail.com"
    def SFDC_HOST_DH="https://asiftech-dev-ed.my.salesforce.com"
    def CONNECTED_APP_CONSUMER_KEY_DH="3MVG9Y6d_Btp4xp5jXRtDdzWrG0fMO9_ITSbNJE7MQoiYm1MV_N3OxMDvKBDJOCclIGX_MaMIsBjICCM2nlOG"
    def JWT_CRED_ID_DH="e7f812f5-36e5-4c5f-801c-c283f19cf01c"

    println 'KEY IS' 
   // println JWT_KEY_CRED_ID
    println HUB_ORG_DH
    println SFDC_HOST_DH
    println CONNECTED_APP_CONSUMER_KEY_DH
    def toolbelt = tool 'sfdx'

    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }
    
    withCredentials([file(credentialsId: JWT_CRED_ID_DH, variable: 'jwt_key_file')]) {
        stage('Deploy Code') {
            if (isUnix()) {
             sh   rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant -u ${HUB_ORG_DH} -f \"${jwt_key_file}\" -i ${CONNECTED_APP_CONSUMER_KEY_DH} -r ${SFDC_HOST_DH}"

            }else{
             sh    rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant -u ${HUB_ORG_DH} -f \"${jwt_key_file}\" -i ${CONNECTED_APP_CONSUMER_KEY_DH} -r ${SFDC_HOST_DH}"

            }
            if (rc != 0) { error 'hub org authorization failed' }

			println rc
			
			// need to pull out assigned username
			if (isUnix()) {
			sh	rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${HUB_ORG_DH}"
			}else{
			   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${HUB_ORG_DH}"
			}
			  
            printf rmsg
            println('Hello from a Job DSL script!')
            println(rmsg)
        }
    }
    def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
        return bat(returnStatus: true, script: script);
    }
}