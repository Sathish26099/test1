pipeline {
    agent any
    
    stages{
        stage('pull git'){
        steps {
            git 'https://github.com/Sathish26099/test1'
        }
        
        }
        
        stage('Build image'){
        steps{
            sh 'sudo docker build -t myimg /var/lib/jenkins/workspace/server/'
            sh 'sudo docker tag myimg sathish7164879/myimg:latest'
            sh 'sudo docker tag myimg sathish7164879/myimg:${BUILD_NUMBER}'
        }
        }
        stage('push image'){
        steps{
            withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    // Login to Docker Hub
                    sh 'echo "$DOCKER_PASSWORD" | sudo docker login -u "$DOCKER_USERNAME" --password-stdin'
            sh 'sudo docker push sathish7164879/myimg:latest'
            sh 'sudo docker push sathish7164879/myimg:${BUILD_NUMBER}'
        
        }
        }
    }
        stage('deployement'){
        steps{
            withCredentials([file(credentialsId: 'con', variable: 'KUBECONFIG')]) {
            sh 'kubectl apply -f /var/lib/jenkins/workspace/server/pod.yaml --validate=false'
            sh 'sudo kubectl rollout restart deployment load-pod'
        }
}
}
}
}
        
        
        
