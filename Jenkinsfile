pipeline {
    agent any
    
    stages {
        stage('install-pip-deps') {
            steps {
                script {
                  installDependencies()
                }
            }
        }
        stage('deploy-to-dev') {
            steps {
                script {
                  deployApp('dev', '7001') 
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script {
                  runTests('dev')
                }
            }
        }
        stage('deploy-to-stg') {
            steps {
                script { 
                  deployApp('stg', '7002')
                }
            }
        }
        stage('tests-on-stg') {
            steps {
                script {
                  runTests('stg')
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script {
                  deployApp('preprod', '7003')
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script {
                  runTests('preprod')
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script { 
                  deployApp('prod', '7004')
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script {
                  runTests('prod') 
                }
            }
        }
    }
}

def installDependencies() {
    echo 'Installing all required dependencies...'
    git branch:'main', url: 'https://github.com/mtararujs/python-greetings'
    sh 'ls'
    sh 'python3 -m venv venv'
    sh './venv/bin/python -m pip install -r requirements.txt'
}

def deployApp(String envName, String port) {
    echo "Deploying application to ${envName} environment on port ${port}..."
    git branch:'main', url: 'https://github.com/mtararujs/python-greetings'
    sh 'pm2 delete greetings_${envName} & set "errorlevel=0"'
    sh "pm2 start app.py --name greetings_${envName} --interpreter ./venv/bin/python -- --port ${port}"
}

def runTests(String envName) {
    echo "Running tests on ${envName} environment..."
    git branch:'main', url: 'https://github.com/mtararujs/course-js-api-framework'
    sh 'npm install'
    sh "npm run greetings greetings_${envName}"
}
