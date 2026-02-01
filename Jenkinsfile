
pipeline {
    agent any

    environment {
        // Ruta completa de Maven en Windows
        MAVEN_HOME = 'C:\\tools\\maven\\apache-maven-3.9.12\\bin'
        // Ruta del JAR generado
        JAR_FILE = 'target\\demo-app-0.0.1-SNAPSHOT.jar'
        // Datos de despliegue
        DEPLOY_USER = 'deploy'
        DEPLOY_HOST = '192.168.0.77'
        DEPLOY_PATH = '/opt/my-app/'
        // Ruta del archivo de identidad SSH si usas clave privada
        SSH_KEY = 'C:\\Users\\llano\\.ssh\\id_rsa_deploy' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo "📥 Checkout del repositorio"
                git url: 'https://github.com/Jhonjairito25/springboot-lab4-deploy.git', branch: 'main', credentialsId: 'deploy-ssh-key'
            }
        }

        stage('Build') {
            steps {
                echo "🔨 Compilando con Maven"
                bat "\"${env.MAVEN_HOME}\\mvn.cmd\" clean package -DskipTests"
            }
        }

        stage('Deploy to VM') {
            steps {
                echo "🚀 Copiando JAR a la VM"
                // Usando SCP con clave SSH
                bat "scp -i ${env.SSH_KEY} ${env.JAR_FILE} ${env.DEPLOY_USER}@${env.DEPLOY_HOST}:${env.DEPLOY_PATH}"
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completado correctamente"
        }
        failure {
            echo "❌ Algo falló en el pipeline. Revisa los logs"
        }
    }
}
