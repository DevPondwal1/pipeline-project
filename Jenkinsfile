pipeline {
    agent any

    environment {
        APP_DIR = "/var/www/html"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/DevPondwal1/pipeline-project.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
                
                // Example for Node.js project
                sh '''
                if [ -f package.json ]; then
                    npm install
                    npm run build
                fi
                '''
            }
        }

        stage('Deploy Website') {
            steps {
                echo "Deploying website to server..."

                sh '''
                sudo rm -rf ${APP_DIR}/*
                
                # If React/Vue/Angular build folder exists
                if [ -d dist ]; then
                    sudo cp -r dist/* ${APP_DIR}/
                elif [ -d build ]; then
                    sudo cp -r build/* ${APP_DIR}/
                else
                    # Static HTML website
                    sudo cp -r * ${APP_DIR}/
                fi
                '''
            }
        }

        stage('Restart Web Server') {
            steps {
                sh '''
                sudo systemctl restart nginx || sudo systemctl restart apache2
                '''
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
