pipeline{
    agent any
    stages{
        stage('Download'){
            steps{
                git branch: 'main', url: 'https://github.com/Arjun-Vinod/end-to-end-devops-project.git'
            }
        }
        stage('Create Docker Images'){
            steps{
                sh 'cd /var/lib/jenkins/workspace/project-end-to-end/vote; docker build -t arjunvinod77/voting-app .'
                sh 'cd /var/lib/jenkins/workspace/project-end-to-end/worker; docker build -t arjunvinod77/worker-app .'
                sh 'cd /var/lib/jenkins/workspace/project-end-to-end/result; docker build -t arjunvinod77/result-app .'
            }
        }
        stage('Push Docker images to Docker Registry'){
            steps{
                sh 'docker push arjunvinod77/voting-app'
                sh 'docker push arjunvinod77/worker-app'
                sh 'docker push arjunvinod77/result-app'
            }
        }
        stage('Deploy to QA servers as containers'){
            steps{
                withCredentials([
                    string(credentialsId: 'DB_USER', variable: 'DB_USER'),
                    string(credentialsId: 'DB_PASS', variable: 'DB_PASS')
                ]){
                    sh 'ssh ubuntu@172.31.23.186 ansible-playbook deploy-containers-qa.yml -e "DB_USER=$DB_USER" -e "DB_PASS=$DB_PASS" -b'
                }
            }
        }
        stage('Download and run selenium scripts'){
            steps{
                git branch: 'main', url: 'https://github.com/Arjun-Vinod/WebAppTesting.git'
                sh 'java -jar /var/lib/jenkins/workspace/project-end-to-end/testing.jar '  
            }
        }
        stage('Deploy into Kubernetes cluster'){
            steps{
                sh '''
                    ssh ec2-user@172.31.27.204 "
                        kubectl apply -f k8s/deployments &&
                        kubectl apply -f k8s/services
                    "
                '''
            }   
        }
    }
}