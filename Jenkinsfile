
pipeline{

    agent any
    environment{
        workspace = "/var/www/stage"
    }

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

