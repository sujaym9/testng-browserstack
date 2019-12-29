
def setupEnv() {
    ["PATH+MAVEN=${tool 'maven3'}/bin",
     "PATH+JAVA_HOME=${tool 'jdk1.8'}/bin"
     ]
}


def notifyFailed(Exception e, boolean failBuild, boolean isBDD, String failDescription) {
    String buildResult = (failBuild && !isBDD) ? "FAILED" : "UNSTABLE";
    currentBuild.result = buildResult;
    slackBuildColor = failBuild ? '#FF0000' : '#fffc66';
    slackSend(color: slackBuildColor, channel: '#qa_test', message: currentBuild.result + ": Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
   

    throw e;
}





def notifyStart() {
    slackSend(color: '#FFFF00', channel: '#qa_test', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
}

node {

browserstack('978951c6-edd7-4beb-b45c-ea2fb6857971') {
    
	 try{
        notifyStart()
		
        timestamps {
                       stage('test') {
                withEnv(setupEnv()) {
                    checkout scm
                                       

                    try {
                        bat 'mvn test -P single'
      
                    }
                    catch (Exception e) {
                        notifyFailed(e, true, false, "Compile failure");
                    }
                }
            }
            
            
            
        }
    } finally {
        echo 'Closing Jenkins Jobs'
       
    }
}

   
}
 
 

