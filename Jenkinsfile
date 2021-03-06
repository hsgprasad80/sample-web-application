pipeline{

  agent any
  triggers {
    githubPush()
  }
        
        stages{

              stage('Quality Gate Status Check'){
                  steps{
                      script{
		    	    sh "mvn clean install"
                 	}

               	 }  
              }	
		
            }	       	     	         
}
