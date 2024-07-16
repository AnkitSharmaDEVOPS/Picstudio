pipeline {
    agent any

    environment {
        SERVER_ADDRESS = 'ubuntu@172.31.33.29'
    }

    stages {
        stage('Cloning the Code') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'Github_Creds', poll: false, url: 'https://github.com/AnkitSharmaDEVOPS/Picstudio.git'
            }
        }
        stage('Sending files to Docker Server with Dockerfile') {
            steps {
                sshagent(['Docker_creds']) {
                    sh 'scp -o StrictHostKeyChecking=no -r * ${SERVER_ADDRESS}:/home/ubuntu/picstudio'
                } 
            }
        }
        stage('BUilding and Deploying the container') {
             steps {
                sshagent(['Docker_creds']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ${SERVER_ADDRESS} << EOF
                    cd  /home/ubuntu/picstudio
                    docker build -t ps_${env.BUILD_NUMBER} .
                    docker run -d --name ps_${env.BUILD_NUMBER} -p 8080:80 ps_${env.BUILD_NUMBER}
EOF
                    """
                } 
            }
        }

    }//stagesclosing
}//pipelineclosing
