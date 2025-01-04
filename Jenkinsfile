node {
    def app  // Variable pour stocker l'objet image Docker

    // Étape 1 : Clone du référentiel
    stage('Clone repository') {
        checkout scm
    }

    // Étape 2 : Construction de l'image Docker
    stage('Build image') {
        app = docker.build("loubnakarim2001268/jenkins-flask")
    }

    // Étape 3 : Tester l'image Docker
    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'  // Vous pouvez ajouter des scripts de test réels ici
        }
    }

    // Étape 4 : Pousser l'image Docker sur Docker Hub
    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")  // Pousse l'image avec un tag de version
        }
    }
    
    // Étape 5 : Déclenchement d'un autre job Jenkins
    stage('Trigger manifest pipeline') {
        echo "Triggering the manifest pipeline job"
        build job: 'manifest pipeline', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
}

