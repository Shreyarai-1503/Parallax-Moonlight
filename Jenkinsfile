CODE_CHANGES = getGitChanges() // Assume this function returns true if there are code changes, false otherwise
def script


pipeline{
    agent any
    environment {
        NEW_VERSION = '1.0.0'
        // SERVER_CREDENTIALS = credentials('server-credentials-id')
    }
    tools {
        maven 'Maven 3.6.3'//name of the maven tool configured in Jenkins global tools configuration
    }
    parameters {
        string(name: 'NEW_VERSION', defaultValue: '1.0.0', description: 'New version to be built')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'staging', 'prod'], description: 'Deployment environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Whether to run tests')
    }
    stages{
        stage('init'){
            steps {
                echo 'Initializing...'
                script {
                    // Call the function defined in script.groovy
                    script = load 'script.groovy'
                }
            }
        }
        stage('build'){
            when {
                expression {
                    //boolean condition
                    env.BRANCH_NAME == 'dev' && CODE_CHANGES == true
                }
            }
            steps{
                script {
                    def var : 2+2>3 ? "cool":"not cool"
                    script.build()
                }
                echo 'Building...'
                echo "New version: ${NEW_VERSION}"
            }
        }
        stage('test'){
            when {
                expression {
                    //boolean condition
                    env.BRANCH_NAME == 'dev' || env.BRANCH_NAME == 'main'
                    params.RUN_TESTS == true
                }
            }
            steps{
                script{
                    script.test()
                }
                echo 'Testing...'
            }
        }
        stage('deploy'){
            steps{
                echo 'Deploying...'
                withCredentials([
                    usernamePassword(credentialsId: 'server-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')
                ]){
                    sh "some script --username ${USERNAME} --password ${PASSWORD}"
                }
                echo "Deploying version: ${params.NEW_VERSION}"
            }
        }
    }
}