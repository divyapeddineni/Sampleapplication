node{
    stage("Git Clone"){
        git 'https://github.com/divyapeddineni/Sampleapplication.git'
    }
    stage("Maven Clean Build"){
        sh 'mvn clean package'
    }
    stage('Build Docker Image'){
        sh 'docker build -t 102402/javaapp1:${BUILD_NUMBER} .'
    }
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'DOCKER_HUB_PASSWORD')]) {
          sh "docker login -u 102402 -p ${DOCKER_HUB_PASSWORD}"
        }
        sh 'docker push 102402/javaapp1:${BUILD_NUMBER}'
     }
     stage("Deploy To Kubernetes Cluster"){
        sh 'kubectl apply -f javaapp.yml'
        sh 'kubectl set image deployments/java-deployment javawebapp=102402/javaapp1:${BUILD_NUMBER}'
      } 
}
