pipeline {
    
    agent {label 'build-node-app'} 
    environment {
        registry = "411636737021.dkr.ecr.eu-west-3.amazonaws.com/mydocker-ec2-ecr"
    }
    stages {
        
        stage ('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mjsanthosh/myPythonDockerRepo.git']]])
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build registry
                }
                
            }
        }
        stage('Docker push') {
            steps {
                script {
                    sh 'aws ecr get-login-password --region eu-west-3 | docker login --username AWS --password-stdin 411636737021.dkr.ecr.eu-west-3.amazonaws.com'
                    sh 'docker push 411636737021.dkr.ecr.eu-west-3.amazonaws.com/mydocker-ec2-ecr:latest'
                }
            }
        }
        stage ('Remove containers') {
            steps {
                script {
                    sh 'docker rm -f $(docker ps -q)'
                }
            }
        }
        stage('Docker run') {
            steps {
                script {
                    sh 'docker run -d -p 8096:5000 --rm --name mypythoncontainer 411636737021.dkr.ecr.eu-west-3.amazonaws.com/mydocker-ec2-ecr'
                }
            }
        }
    }
    post {
        always {
            mail to: "doodlebluetest07@gmail.com",
            subject: "jenkins build docker image push to ecr",
            body: "push the image to ecr is successfull"
        }
    }
}
