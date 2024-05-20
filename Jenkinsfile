pipeline {
  agent any
    tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CRD', url: 'https://github.com/RushiPatil1881/hello.git']])
                }
            }
        }
     
  
    stage('Build Docker Image') {
        steps {
            script {
                sh 'docker build -t rushidockerhub1881/todayapp .'
                
            }
            
        }
        
    }


    stage('Deploy Docker Image to DockerHub') {
        steps {
            script {
                withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_CRD', passwordVariable: 'passwordDocker', usernameVariable: 'usernameDocker')])
                {
                    sh 'docker login -u ${usernameDocker} -p ${passwordDocker}'
                    
                }
                sh 'docker push rushidockerhub1881/todayapp:latest'
                
            }
        }
        
    }    
    
    stage('eks cluster setup on jenkins') {
        steps {
            script{
                sh ('aws eks update-kubeconfig --name eks1881 --region ap-south-1')
                sh "kubectl get ns"
                }
            
        }     
    }   
    
    
    stage('Deploying helloapp to Kubernetes') {
        steps {
            script 
            {
                sh "kubectl apply -f nodejsapp.yaml"
                
            }
            
        }
        
        
    }
      
  }
    
}
