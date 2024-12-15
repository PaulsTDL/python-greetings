pipeline {
    agent any
    stages {
        stage('build-docker-image') {
            steps {
                build()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy("DEV")
            }
        }
        stage('tests-on-dev') {
            steps {
                test("DEV")
            }
        }
        stage('deploy-to-stg') {
            steps {
                deploy("STG")
            }
        }
        stage('tests-on-stg') {
            steps {
                test("STG")
            }
        }
        stage('deploy-to-prod') {
            steps {
                deploy("PRD")
            }
        }
        stage('tests-on-prod') {
            steps {
                test("PRD")
            }
        }
    }
}

def build(){
    echo "Building docker image..."
    sh "docker build -t pgreinhards/python-greetings-app:latest ."

    echo "Pushing image to docker registry.."
    sh "docker push pgreinhards/python-greetings-app:latest"
}

def deploy(String environment){
    echo "Deploying Python microservice to ${environment} environment.."
    sh "docker pull pgreinhards/python-greetings-app:latest"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"
    sh "docker-compose up greetings-app-${environment}"
}

def test(String environment){
    echo "Running API tests on ${environment} environment.."
    sh "docker pull pgreinhards/api-tests:latest"
    sh "docker run --network=host --rm pgreinhards/api-tests:latest run greetings greetings_${environment}"
}