
// Pipeline creation

pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['test', 'dev', 'qa', 'prod'],
            description: 'Select environment'
        )
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Read Configuration from JSON') {
            steps {
                script {
                    def cfg = readJSON file: 'config.json'

                    def envCfg = cfg[params.ENVIRONMENT]

                    env.APP_NAME = envCfg.app_name
                    env.SERVER   = envCfg.server
                    env.PORT     = envCfg.port.toString()
                    
            env.REGIONS = ''
            envCfg.regions.each { r ->
                env.REGIONS += r + ','
            }
            env.REGIONS = env.REGIONS[0..-2]


                    echo "Environment : ${params.ENVIRONMENT}"
                    echo "App Name    : ${env.APP_NAME}"
                    echo "Server      : ${env.SERVER}"
                    echo "Port        : ${env.PORT}"
                    echo "Regions     : ${env.REGIONS}"
                }
            }
        }

        stage('Deploy') {
            steps {
                sh """
                    echo "Deploying ${APP_NAME}"
                    echo "Server : ${SERVER}"
                    echo "Port   : ${PORT}"
                    echo "Regions: ${REGIONS}"
                    echo "Deployment SUCCESS"
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}