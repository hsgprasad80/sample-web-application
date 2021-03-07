pipeline{

  agent any
  triggers {
    githubPush()
  }
        
        stages{
	
              stage('Quality Gate Status Check'){
		        agent {
                docker {
                image 'maven'
                args '-v $HOME/.m2:/root/.m2'
		       }
		   }
                  steps{
                      script{
                      withSonarQubeEnv('sonarserver') { 
                      sh "mvn sonar:sonar"
                       }
                      
		    	    sh "mvn clean install"
                 	}

               	 }  
              }	
		
            }	       	     	         
}
