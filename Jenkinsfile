pipeline { 

    environment { 

        registry = "than97/pvpkg" 

        registryCredential = '9a70e374-ebc8-4705-a7dd-d358c5549519' 

        dockerImage = '' 
    }
    agent any 

    stages { 


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

