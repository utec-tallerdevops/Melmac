pipeline { 
		agent none 
			stages { 
				stage('Build imagenes') {
					 agent { 
						node { 
							label 'master' 
							    } 
						    }
        					steps {
        						 // La clausula dir hace que todas las instrucciones se 
        						 // ejecuten relativas a ese directorio. 
        						dir('worker'){ 
        							sh 'docker build -t devopsutec.azurecr.io/melmac-voteapp-worker:22 .'
        							}
        						dir('result'){
        							sh 'docker build -t devopsutec.azurecr.io/melmac-voteapp-results:4 .' 
        							}
        						dir('vote'){
        							sh 'docker build -t devopsutec.azurecr.io/melmac-voteapp-vote:3 .'
        							}
						    } 
				    	}
				stage('Push imagenes'){
					 agent { 
					 	node { 
					 		label 'master' 
					 	} 
					 } 
				steps{ 
        				// Usar la cláusula withDockerRegistry para que docker 
        				// se autentique contra el registro de imagenes.
        				withDockerRegistry(credentialsId: 'MelmacACR', url:'https://devopsutec.azurecr.io'){ 
        				sh 'docker push devopsutec.azurecr.io/melmac-voteapp-worker:${BUILD_NUMBER}'
        				sh 'docker push devopsutec.azurecr.io/melmac-voteapp-results:${BUILD_NUMBER}'
        				sh 'docker push devopsutec.azurecr.io/melmac-voteapp-vote:${BUILD_NUMBER}'
				 	}
    			 }

			}
				 stage('Deploy compose') { 
				 	agent { 
				 		node { 
				 			label 'utec' 
				 			} 
				 		}
        				 steps { 
        				     dir('./'){
        				         sh 'docker-compose up -d'
        				            }
        						} 
			        }
		      }
    }
