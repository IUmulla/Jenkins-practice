pipeline {
    agent any
    stages {
        stage {'build maven'}{
            steps {
                sh 'pwd'
                sh 'mvn clean install pakage'
            }
        }
        stage {'copy from artifacts'}{
            step {
                sh 'pwd'
                sh 'cp -r taget/*.jar docker'
            }
        }
        stage {'build docker image'}{
            steps {
                script {
                    def customeimage = docker.build("iamsakib/petclinic:${env.BUILD_NUMBER}", "./docker")
                    docker.withRegistry('https://.hub.docker.com', 'dockerhub') {
                    customImage.push() 
                    }
                }

            }
        
        }
        stage {'build on kubernetes'} {
            step {
                withKubeConfig{[credentialsId: 'kubeconfig']}{
                    sh 'pwd'
                    sh 'cp -R help/* .'
                    sh 'ls -lrth'
                    sh 'pwd'
                    sh 'user/local/bin/helm upgarde --install petclinic-app petclinic --set image.tag=${BUILD_NUMBER}'
                }
                }
            }
        }

    }
