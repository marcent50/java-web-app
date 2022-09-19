pipeline {
 agent { label 'linux' }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  } 
  environment {
     registry = "marcent/java-app" 
     registryCredential = 'token-jenkins'
     dockerImage = '' 
  }
  parameters { 
    string(name: 'APP_NAME', defaultValue: '', description: 'What is the Heroku app name?') 
  }
agent any 

stages { 

    stage('Cloning our Git') { 

        steps { 

            git 'https://github.com/marcent50/Docker_1.git' 

        }

    } 
    
     stage('Initialize') {
         
        steps{
            
            script {
                
                  def dockerHome = tool 'mydocker'
                   
                env.PATH = "${dockerHome}/bin:${env.PATH}"
                
                }

        } 

    }

    stage('Building our image') { 

        steps { 

            script { 

                dockerImage = docker.build registry + ":$BUILD_NUMBER"

            }

        } 

    }
    
   
        stage('Deploy our image') { 

        steps { 

            script { 

                docker.withRegistry( '', registryCredential ) {

                    dockerImage.push() 

                }

            } 

        }

    } 
    

    stage('Cleaning up') { 

        steps { 

            sh "docker rmi $registry:$BUILD_NUMBER" 

        }

    } 

}
}
