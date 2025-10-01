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
    }
}