pipeline {
    agent any

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                dir('STUDENTAPI-REACT') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                sh '''
                FRONTEND_DIR="/Users/rishi/tomcat10/webapps/reactstudentapi"

                if [ -d "$FRONTEND_DIR" ]; then
                    rm -rf "$FRONTEND_DIR"
                fi

                mkdir "$FRONTEND_DIR"
                cp -R STUDENTAPI-REACT/dist/* "$FRONTEND_DIR"
                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                dir('STUDENTAPI-SPRINGBOOT') {
                    sh '/Users/rishi/Desktop/OddSem/apache-maven-3.9.11/bin/mvn clean package'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                sh '''
                BACKEND_WAR="/Users/rishi/tomcat10/webapps/springbootstudentapi.war"
                BACKEND_DIR="/Users/rishi/tomcat10/webapps/springbootstudentapi"

                if [ -f "$BACKEND_WAR" ]; then
                    rm -f "$BACKEND_WAR"
                fi

                if [ -d "$BACKEND_DIR" ]; then
                    rm -rf "$BACKEND_DIR"
                fi

                cp STUDENTAPI-SPRINGBOOT/target/*.war /Users/rishi/tomcat10/webapps/
                '''
            }
        }

    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Pipeline Failed.'
        }
    }
}
