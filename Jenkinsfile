pipeline {
    agent any

    tools {
        maven 'Maven'  // Make sure this matches the name in Jenkins > Global Tool Configuration
    }

    environment {
        ANYPOINT_CREDENTIALS = credentials('Anypoint_credentials')
        MULE_ENV = 'dev'  // or any other env variable you want to use
    }

    stages {

        stage('Build') {
            steps {
                echo "🔨 Building project..."
                sh 'mvn -B -U -e -V clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running unit tests..."
                sh 'mvn test'
            }
        }

        stage('Deployment') {
            environment {
                CLIENT_ID     = credentials('dev_client_id')
                CLIENT_SECRET = credentials('dev_client_secret')
            }
            steps {
                echo "🚀 Deploying to CloudHub (${MULE_ENV})..."
                sh """
                    mvn -U -V -e -B -DskipTests deploy -P${MULE_ENV} -DmuleDeploy \
                    -Dusername=\$ANYPOINT_CREDENTIALS_USR \
                    -Dpassword=\$ANYPOINT_CREDENTIALS_PSW \
                    -Danypoint.platform.client_id=\$CLIENT_ID \
                    -Danypoint.platform.client_secret=\$CLIENT_SECRET
                """
            }
        }
    }
}
  
