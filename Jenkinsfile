pipeline {
 
 agent any
     
    stages {
        stage('Getting Project from Git') {
            steps {
                echo 'Project is downloading...'
                git branch:'main', url:'https://github.com/DhouhaAR/MLprod.git'
  
                 }
             }
          stage('Building docker container') {
            steps {
                  sh 'docker build -t my-kube-api .'
                  sh 'docker run -d -p 5000:5000 my-kube-api python3 api.py'
               }
           }
            
       
       /* stage('Building docker image') {
            steps {
                  sh 'docker logout '
                  sh 'docker build -t diabetes-model2 .'
                  sh 'docker tag diabetes-model2 dhouha20/diabetes-model2:diabetes-model2'
                  
               }
           }*/
             stage('Pushing Image') {
      
      steps{
        script {
           //docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
           sh 'docker login -u "dhouha20" -p "94257005Dhouha" docker.io'
           sh 'docker tag my-kube-api:latest dhouha20/my-kube-api:latest'
           sh 'docker push dhouha20/my-kube-api:latest'
          
        }
      }
    }
    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", kubeconfigId: "kubernetes")
        }
      }
    }
    
    }
}
