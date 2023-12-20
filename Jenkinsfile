pipeline {
    agent any
    
    environment {
        // Définissez les variables d'environnement nécessaires
        DOCKER_REGISTRY = 'votre_registry_docker'
    }
    
    stages {
        stage('Detect Language') {
            steps {
                script {
                    // Détection automatique du langage en fonction des fichiers présents
                    if (fileExists('Makefile')) {
                        LANGUAGE = 'C'
                    } else if (fileExists('pom.xml')) {
                        LANGUAGE = 'Java'
                    } else if (fileExists('package.json')) {
                        LANGUAGE = 'JavaScript'
                    } else if (fileExists('requirements.txt')) {
                        LANGUAGE = 'Python'
                    } else if (fileExists('main.bf')) {
                        LANGUAGE = 'Befunge'
                    } else {
                        echo "Langage non détecté. Assurez-vous que les fichiers appropriés sont présents."
                        currentBuild.result = 'FAILURE'
                        return
                    }
                    echo "Langage détecté : \${LANGUAGE}"
                }
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Construit et pousse l'image Docker en fonction du langage du projet
                    script.sh """
                        case "\${LANGUAGE}" in
                            "C")
                                docker build -t \${DOCKER_REGISTRY}/whanos-c:\${BUILD_NUMBER} -f images/c/Dockerfile.base .
                                ;;
                            "Java")
                                docker build -t \${DOCKER_REGISTRY}/whanos-java:\${BUILD_NUMBER} -f images/java/Dockerfile.base .
                                ;;
                            "JavaScript")
                                docker build -t \${DOCKER_REGISTRY}/whanos-javascript:\${BUILD_NUMBER} -f images/javascript/Dockerfile.base .
                                ;;
                            "Python")
                                docker build -t \${DOCKER_REGISTRY}/whanos-python:\${BUILD_NUMBER} -f images/python/Dockerfile.base .
                                ;;
                            "Befunge")
                                docker build -t \${DOCKER_REGISTRY}/whanos-befunge:\${BUILD_NUMBER} -f images/befunge/Dockerfile.base .
                                ;;
                            *)
                                echo "Langage non pris en charge."
                                currentBuild.result = 'FAILURE'
                                return
                                ;;
                        esac
                        
                        docker push \${DOCKER_REGISTRY}/whanos-\${LANGUAGE}:\${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}
