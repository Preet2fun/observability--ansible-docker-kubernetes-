pipeline{
    agent any
    
    environment {
      DOCKER_TAG = getVersion()
    }
    stages{
        stage('SCM'){
            steps{
                git branch: 'main', credentialsId: 'bcbb5862-127d-441b-86da-626ea837175d', 
                url: 'https://github.com/Preet2fun/observability-ansible-docker-kubernetes.git'
            }
        }
                
        stage('Docker Build App'){
            steps{
                //sh "cd app/"
                sh "docker build /var/lib/jenkins/workspace/docker-ansible-app-cicd/app -t preet2fun/mainapp:${DOCKER_TAG} "
            }
        }

        stage('Docker Build Web'){
            steps{
                //sh "cd web/"
                sh "docker build /var/lib/jenkins/workspace/docker-ansible-app-cicd/web -t preet2fun/webnginxapp:${DOCKER_TAG} "
            }
        }
        
        stage('DockerHub Push'){
            steps{
                //withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')])
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u preet2fun -p ${dockerHubPwd}"
                }
                
                sh "docker push preet2fun/mainapp:${DOCKER_TAG} "
                sh "docker push preet2fun/webnginxapp:${DOCKER_TAG} "
            }
        }
        /*
        stage('Docker Deploy'){
            steps{
              ansiblePlaybook credentialsId: 'dev-server', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${DOCKER_TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
        */
    }
}

def getVersion(){
    def commitHash = sh label: '', returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}