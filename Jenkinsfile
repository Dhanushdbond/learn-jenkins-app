pipeline {
    agent any

    stages {
        stage('Build React App') {
            agent {
                docker {
                    image 'node:18'
                    args '-u root:root'  // prevents permission issues
                    reuseNode true
                }
            }
            steps {
                echo ' Starting React build...'
                sh '''
                    set -e
                    ls -la
                    node --version
                    npm --version
                    
                    # Clean environment
                    rm -rf node_modules
                    
                    echo " Installing dependencies..."
                    if ! npm ci; then
                        echo " npm ci failed — running npm install instead..."
                        npm install
                    fi

                    echo " Building project..."
                    npm run build
                '''
            }
        }

        stage('Archive Build') {
            steps {
                echo ' Archiving build artifacts...'
                archiveArtifacts artifacts: 'build/**', fingerprint: true
            }
        }
    }

    post {
        success {
            echo ' Build completed successfully!'
        }
        failure {
            echo ' Build failed. Check console output for errors.'
        }
    }
}
