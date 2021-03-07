def getDockerTag(){
        def tag = sh script: 'git rev-parse HEAD', returnStdout: true
        return tag
        }

pipeline{   

  agent any

  triggers {
    githubPush()
    }
  environment{
	    Docker_tag = getDockerTag()
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
              stage('build')
                {
              steps{
                  script{
		               sh 'cp -r ../devops-training@2/target .'
                   sh 'docker build . -t hsgprasad80/devops-training:$Docker_tag'
		               withCredentials([string(credentialsId: 'Dockerhub', variable: 'Dockerhub')]) { 
                         sh 'docker login -u hsgprasad80 -p $Dockerhub'
				                 sh 'docker push hsgprasad80/devops-training:$Docker_tag'
			                    } 
                        } 
                    }
                }	
    stage('ansible playbook'){
			steps{
			 	script{
				    sh '''final_tag=$(echo $Docker_tag | tr -d ' ')
				     echo ${final_tag}test
				     sed -i "s/docker_tag/$final_tag/g"  deployment.yaml
				     '''
				    ansiblePlaybook become: true, becomeUser: 'ansible', installation: 'ansible', inventory: 'hosts', playbook: 'ansible.yml'
				}
			}
		}	
            }
}
