 pipeline { 
    agent any

    environment { 
        // define environment variable
        // Jenkins credentials configuration
        DOCKER_HUB_CREDENTIALS = credentials('1') // Docker Hub credentials ID store in Jenkins
        // Docker Hub Repository's name
        DOCKER_IMAGE = 'wenyunxiang/teedy' // your Docker Hub user name and Repository's name
        DOCKER_TAG = "${env.BUILD_NUMBER}" // use build number as tag 
    tools {
    maven '3.9'
}
    environment { 
        // define environment variable
        // Jenkins credentials configuration
        DOCKER_HUB_CREDENTIALS = credentials('my-dockerhub-id') // Docker Hub credentials ID store in Jenkins
        // Docker Hub Repository's name
        DOCKER_IMAGE = 'ubuntu' // your Docker Hub user name and Repository's name
        DOCKER_TAG = "22.04" // use build number as tag 
        DOCKER_PATH = '/usr/local/bin/docker'
    } 
    stages { 
        stage('Build') { 
            steps { 
                checkout scmGit( 
                    branches: [[name: '*/master']],
                    extensions: [],  
                    userRemoteConfigs: [[url: 'https://github.com/Java-wyx/Teady.git', credentialsId: '66deff23-9ee2-431b-a67d-856dac706302']] // your github Repository
                ) 
                sh 'mvn -B -DskipTests clean package'
            } 
        } 
        // Building Docker images
        stage('Building image') { 
            steps { 
                script { 
                    // assume Dockerfile locate at root
                    docker.build("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}") 
                } 
            } 
        } 
        // Uploading Docker images into Docker Hub
        stage('Upload image') { 
            steps { 
                script { 
                    // sign in Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'my-dockerhub-id') {
            // push image
docker.image("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}").push() 
            // ：optional: label latest
docker.image("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}").push('latest') 
                    } 
                }
         }
    }
// Running Docker container
        stage('Run containers') {
            steps {
                script {
                // stop then remove containers if exists
                sh 'docker stop teedy-container-8081 || true'
                sh 'docker rm teedy-container-8081 || true'
                // run Container
                docker.image("${env.DOCKER_IMAGE}:${env.DOCKER_TAG}").run('--name teedy-container-8081 -d -p 8081:8080')
                // Optional: list all teedy-containers
                sh 'docker ps --filter "name=teedy-container"'
                }
            }
        }
    }
}
}
