pipeline{
    agent any
    environment{
        DOCKER_USERNAME = 'dat1edf'
        TOKEN_ISSUER = credentials('TOKEN_ISSUER')
        TOKEN_AUDIENCE = credentials('TOKEN_AUDIENCE')
        TOKEN_KEY = credentials('TOKEN_KEY')
    }

    stages{
        stage ('Deploy APIs'){
            steps{
                sh "echo 'Deploying and cleaning'"
                    script {
                        def deploying = """
                        #!/bin/bash
                        export DOCKER_USERNAME=${DOCKER_USERNAME}
                        export TOKEN_ISSUER=${TOKEN_ISSUER}
                        export TOKEN_AUDIENCE=${TOKEN_AUDIENCE}
                        export TOKEN_KEY=${TOKEN_KEY}
                        cd nosh_cicd
                        git pull
                        docker image pull ${DOCKER_USERNAME}/nosh_now_apis
                        docker compose up -d
                        """
                        sshagent (credentials: ['vm-key']) {
                            sh """
                                ssh -o StrictHostKeyChecking=no -i vm-key root@3.27.63.123 "echo '${deploying}' > deploy.sh && chmod +x deploy.sh && ./deploy.sh"
                            """
                        }
                    }
            }
        }
        
    }
}

                        // "docker rm -f api\n"+
                        // "docker rmi ${DOCKER_USERNAME}/nosh_now_api\n"+
                        