pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'SonarQubeScanner' // Nombre configurado en Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Usando el código del workspace local o repositorio Git'
                // Si tienes un repositorio Git, puedes usar:
                // git branch: 'main', url: 'https://github.com/usuario/Proyecto_ACS.git'
            }
        }

        stage('Análisis con SonarQube - CSS') {
            steps {
                withSonarQubeEnv('SonarQubeServer') { // Nombre configurado en "Manage Jenkins > System"
                    bat """
                        "%SCANNER_HOME%\\bin\\sonar-scanner.bat" ^
                        -Dsonar.projectKey=Proyecto_ACS_CSS ^
                        -Dsonar.projectName="Proyecto ACS - CSS" ^
                        -Dsonar.sources=Frontend ^
                        -Dsonar.inclusions=**/*.css ^
                        -Dsonar.exclusions=Frontend\\node_modules/**,Frontend\\dist/** ^
                        -Dsonar.sourceEncoding=UTF-8
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
