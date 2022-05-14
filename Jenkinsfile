pipeline {
    agent {label 'master'}

    stages {
        stage('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/BENHALIMAmohamed/k8s_project.git']]])            }
        }

        stage('Build result') {
        steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                bat 'docker rmi killua1997/result '
                bat 'docker build -t killua1997/result ./result' 
                
            }
        }
        } 
        stage('Build vote') {
        steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                bat 'docker rmi killua1997/vote'
                bat 'docker build -t killua1997/vote ./vote'
            }
        }
        }

        stage('Push result image') {

        steps {
            withDockerRegistry(credentialsId: 'ec9ce312-560f-4ef5-95f7-8ee3a5ede8fd',
             url: '') {
            bat 'docker push killua1997/result'
            }
        }
        }
        stage('Push vote image') {

        steps {
            withDockerRegistry(credentialsId: 'ec9ce312-560f-4ef5-95f7-8ee3a5ede8fd',
             url: '') {
            bat 'docker push killua1997/vote'
            }
    

        }
        }

    }
    post {
      success {
         emailext  attachLog: true, body: 'image has been deployed succesufully', compressLog: true, subject: 'Docker_status', to: 'benhalimamohamed.ca@gmail.com'
            }
    }

}
