pipeline {
    agent any
tools {
    maven 'Maven'
}

    environment {
        ANYPOINT_CREDENTIALS = credentials('Anypoint_credentials')
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -U -e -V clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo "Running unit tests..."
                bat "mvn test"
            }
        }

        stage('Deployment') {
            environment {
                CLIENT_ID = credentials('dev_client_id')
                CLIENT_SECRET = credentials('dev_client_secret')
            }
            steps {
                echo "Deploying to CloudHub..."
                sh '''
    mvn -U -V -e -B -DskipTests deploy -Pdev -DmuleDeploy \
    -Dusername="$ANYPOINT_CREDENTIALS_USR" \
    -Dpassword="$ANYPOINT_CREDENTIALS_PSW" \
    -Danypoint.platform.client_id="$CLIENT_ID" \
    -Danypoint.platform.client_secret="$CLIENT_SECRET"
'''

            }
        }
    }
}
