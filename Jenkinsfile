pipeline {
    agent any

    environment {
        // Ruta completa de Maven en Windows
        MAVEN_HOME = 'C:\\tools\\maven\\apache-maven-3.9.12\\bin'
        PATH = "${env.MAVEN_HOME};${env.PATH}" // agrega Maven al PATH temporalmente

        // Ruta del JAR generado
        JAR_FILE = 'target\\demo-app-0.0.1-SNAPSHOT.jar'

        // Datos de despliegue
        DEPLOY_USER = 'deploy'
        DEPLOY_HOST = '192.168.0.77'

        // Ruta del archivo de identidad SSH
        SSH_KEY = 'C:\\Users\\llano\\.ssh\\id_rsa'
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
                bat """
                echo Maven version:
                mvn -version
                mvn clean package -DskipTests
                """
            }
        }

        stage('Deploy to VM') {
            steps {
                echo "🚀 Copiando JAR a /tmp de la VM"
                bat """
                C:\\Windows\\System32\\OpenSSH\\scp.exe -i "${SSH_KEY}" "${JAR_FILE}" ${DEPLOY_USER}@${DEPLOY_HOST}:/tmp/app.jar
                """

                echo "🚀 Ejecutando deploy.sh en la VM"
                bat """
                C:\\Windows\\System32\\OpenSSH\\ssh.exe -i "${SSH_KEY}" ${DEPLOY_USER}@${DEPLOY_HOST} "bash /opt/my-app/deploy.sh /tmp/app.jar"
                """
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
