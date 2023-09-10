pipeline {
    agent any

   stages {
        stage('Checkout') {
            steps {
                // Replace your generated pipeline script here 
                 checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vijaynvb/todoapi.git']])
                echo 'successful checkout'
            }
        }

        stage('Build jar and image using Docker file ') {
            steps {
                script {
                    def imageTag = "mahan0227/todomyapi:2.0"
                    docker.build(imageTag, '.')
                    echo 'successful Build Docker Image'
                }
            }
        }
        
         stage('Push to Docker Hub') {
            steps {
                script {
                 // Replace your generated pipeline script here 
                    // This step should not normally be used in your script. Consult the inline help for details.
                    withDockerRegistry(credentialsId: 'ff711f40-2e86-4dd4-b22f-a2f0e2581299', url: 'https://index.docker.io/v1/') {
                    def imageTag = "mahan0227/todomyapi:2.0"
                    docker.image(imageTag).push()
                    echo 'successful Push to Docker Hub'
                    }
                }
            }
        }        
              
       
        stage('Deploy to Kubernetes') {
            steps {
                script {
                // Replace your generated pipeline script here 
                  kubeconfig(credentialsId: '9b6634bb-c8ce-4a9f-9879-1f64c8cd4002', serverUrl: 'https://kubernetes.docker.internal:6443')  {
                    def kubeConfig = readFile 'kubernetes-config.yaml'
                    sh "kubectl apply -f kubernetes-config.yaml"
                    }
               }
            }
        }
    }
    
  }