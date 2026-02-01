pipeline {
    agent any

    environment {
        VM_USER = 'deploy'
        VM_HOST = '192.168.0.77'
        VM_PATH = '/opt/my-app'
        JAR_NAME = 'demo-app-0.0.1-SNAPSHOT.jar'
        SCP_PATH = 'C:\\Windows\\System32\\OpenSSH\\scp.exe'
        SSH_PATH = 'C:\\Windows\\System32\\OpenSSH\\ssh.exe'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Jhonjairito25/springboot-lab4-deploy.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to VM') {
            steps {
                echo "📤 Copiando JAR al servidor remoto..."
                bat "${env.SCP_PATH} target\\${JAR_NAME} ${VM_USER}@${VM_HOST}:${VM_PATH}/"
                
                echo "🚀 Ejecutando deploy remoto..."
                bat "${env.SSH_PATH} ${VM_USER}@${VM_HOST} \"cd ${VM_PATH} && ./deploy.sh ${JAR_NAME}\""
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
