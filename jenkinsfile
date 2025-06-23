
pipeline{

    agent any
    environment{
        workspace = "/var/www/stage"
    }

    parameters{
        file(name: '', description: "upload the testing script in .php or .zip")
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests before deploy?')
    }
    stages{
        stage('clone repo'){
            steps{
                sh 'echo "cloning the repo"'
                git credentialsId:'' , url: ''
            }
        }
        stage('composer install'){
            steps{
                sh 'echo "building the project"'
                dir("${env.workspace}") {
                    sh 'composer install '
                    sh 'npm run prod'
                }
            }
        }
        stage('upload custom test file'){
            when{
                expression{return params.TEST_FILE}
            }
            steps{
                script{
                    sh '''
                    mkdir -p ${DEPLOY_DIR}/tests/Custom
                    cp "$TEST_FILE" ${DEPLOY_DIR/tests/Custom/upload_test.php}
                    '''
                }
            }
        }
        stage('run tests'){
            when{
                expression{ return params.RUN_TESTS}
            }
            steps{
                dir("${env.workspace}"){
                    sh 'php artisan test'
                }
            }
        }
        stage('lavarel cache & migration'){
            steps{
                sh 'echo "lavarel cache & migration"'
                dir("${env.workspace}"){
                    sh 'php artisan migrate'
                    sh 'php artisan migrate:tenant'
                    sh 'npm run build'
                }
            }
        }
        
        stage('restart'){
            steps{
                sh ''' 
                
                sudo systemctl restart php8.2-fpm.service
            
                '''
            }
        }
    }
    post{
        failure{
            echo 'build or test failed'
        }
        success{
            echo 'deployed successfully'
        }
    }
}

