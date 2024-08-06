pipeline{
    agent any
    environment{
        DOCKER_USERNAME = 'dat1edf'
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        TOKEN_ISSUER = credentials('TOKEN_ISSUER')
        TOKEN_AUDIENCE = credentials('TOKEN_AUDIENCE')
        TOKEN_KEY = credentials('TOKEN_KEY')
    }

    stages{
        stage ('Deploy APIs'){
            steps{
                sh "echo 'Deploying and cleaning'"
                    script {
                        def deploying = "#!/bin/bash\n" + 
                        "DOCKER_USERNAME=${DOCKER_USERNAME}\n" +
                        "TOKEN_ISSUER=${TOKEN_ISSUER}\n"+
                        "TOKEN_AUDIENCE=${TOKEN_AUDIENCE}\n" +
                        "TOKEN_KEY=${TOKEN_KEY}\n" +
                        "docker rm -f api\n"+
                        "docker rmi ${DOCKER_USERNAME}/nosh_now_api\n"+
                        "docker-compose up -d"
                        sshagent (credentials: ['your-credential-id']) {
                            sh """
                                ssh -o StrictHostKeyChecking=no root@52.63.154.28 "echo \\\"${deploying}\\\" > deploy.sh && chmod +x deploy.sh && ./deploy.sh"
                            """
                        }
                    }
            }
        }
        
    }
}