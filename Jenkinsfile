
def setupEnv() {
    ["PATH+MAVEN=${tool 'maven3'}/bin",
     "PATH+JAVA_HOME=${tool 'jdk1.8'}/bin"
     ]
}


def notifySlack(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'

    def color

    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }

    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n More info at: ${env.BUILD_URL}"

    slackSend(color: color, channel: '#qa_test', message: msg)
}





node {

browserstack('978951c6-edd7-4beb-b45c-ea2fb6857971') {
    
	 try{
        notifySlack()
		
        timestamps {
		
                    stage('Compile') {
				
                withEnv(setupEnv()) {
				
                    checkout scm
                                       

                    try {
                        bat 'mvn compile'
      
                    }
                    catch (Exception e) {
                        currentBuild.result = 'FAILURE'
						throw e
                    }
                }
            }
				stage('Testing') {
				
                withEnv(setupEnv()) {
                    checkout scm
                                       

                    try {
                        bat 'mvn test -P single'
      
                    }
                    catch (Exception e) {
                        currentBuild.result = 'FAILURE'
						
						throw e
                    }
                }
            }
            
            
            
        }
    } finally {
        notifySlack(currentBuild.result)
       
    }
}

   
}
 
 

