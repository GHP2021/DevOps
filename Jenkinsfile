#!groovy
import groovy.json.JsonSlurperClassic
node {

    def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME
    
    def jwt_key_file="C:\\openssl\\bin\\server.key"
    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println jwt_key_file	
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
    def toolbelt = tool 'toolbelt'
   
    
    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

  
    stage('Deploye Code') {
	     println SFDC_HOST    
	     println jwt_key_file	
             bat "${toolbelt}/sfdx update"
	     rc = bat returnStatus: true, script: "${toolbelt}/sfdx auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --loglevel DEBUG --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            if (rc != 0) { 
		    println 'inside rc 0'
		    error 'hub org authorization failed' 
	    }
		else{
			println 'rc not 0'
		}

	    println rc
			
	    rmsg = bat returnStdout: true, script: "${toolbelt}/sfdx force:source:deploy -x manifest/package.xml -u ${HUB_ORG}"
			  
            printf rmsg
            println('Hello from a Job DSL script!')
            println(rmsg)
        }
    
}
