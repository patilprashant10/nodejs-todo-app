pipeline {
    agent { label 'Dev-Agent node' }
    
    stages{
        stage('Checkout'){
            steps{
                git url: 'https://github.com/patilprashant10/nodejs-todo-app.git', branch: 'master'
            }
        }
        stage('Build'){
            steps{
                sh 'sudo docker build . -t patilprashant10/nodo-todo-app-test:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing...'
                sh 'sudo docker inspect --type=image patilprashant10/nodo-todo-app-test:latest '
            }
        }
        
        stage('Push'){
            steps{
        	     sh "sudo docker login -u patilprashant10 -p dckr_pat_WSdI97yEEw0n90icDyZ4vlmEPqY"
                 sh 'sudo docker push patilprashant10/nodo-todo-app-test:latest'
            }
        }  
        stage('Deploy'){
            steps{
                echo 'deploying on another server'
                sh 'sudo docker stop nodetodoapp || true'
                sh 'sudo docker rm nodetodoapp || true'
                sh 'sudo docker run -d --name nodetodoapp patilprashant10/nodo-todo-app-test:latest'
                sh '''
                ssh -i deployment.pem -o StrictHostKeyChecking=no ubuntu@3.80.24.188 <<EOF
                sudo docker login -u patilprashant10 -p dckr_pat_WSdI97yEEw0n90icDyZ4vlmEPqY
                sudo docker pull patilprashant10/nodo-todo-app-test:latest
                sudo docker stop nodetodoapp || true
                sudo docker rm nodetodoapp || true 
                sudo docker run -d --name nodetodoapp patilprashant10/nodo-todo-app-test:latest
                '''
            }
        }
    }
}
