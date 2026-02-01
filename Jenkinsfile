pipeline {
    agent any

    environment {
        VM_USER = 'deploy'
        VM_HOST = '192.168.0.77'
        VM_PATH = '/opt/my-app'
        JAR_NAME = 'demo-app-0.0.1-SNAPSHOT.jar'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jhonjairito25/springboot-lab4-deploy.git',
                    credentialsId: 'deploy-ssh-key'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to VM') {
            steps {
                // Copiar JAR a la VM
                bat "scp target\\${JAR_NAME} ${VM_USER}@${VM_HOST}:${VM_PATH}/"

                // Ejecutar deploy.sh en la VM
                bat "ssh ${VM_USER}@${VM_HOST} \"cd ${VM_PATH} && ./deploy.sh ${JAR_NAME}\""
            }
        }
    }

    post {
        success {
            echo "✅ Deploy completado correctamente!"
        }
        failure {
            echo "❌ Algo falló en el pipeline. Revisa los logs."
        }
    }
}
