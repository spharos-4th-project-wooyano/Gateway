pipeline {
    agent any
    stages {
        stage('Check') {
            steps {
                git branch: 'develop',credentialsId:'0-shingo', url:'https://github.com/Spharos-final-project-WOOYANO/Gateway'
            }
        }
	stage('Gateway-Secret-File Download'){
	    steps{
		withCredentials([
		    file(credentialsId: 'Gateway-Secret-File', variable: 'gatewaysecret')
		])
	        {
	            sh "cp \$gatewaysecret ./src/main/resources/application-secret.yml"
	    	}
	    }
	}
        stage('Build'){
            steps{
                script {
                    sh '''
                        pwd
                        chmod +x ./gradlew
                        ./gradlew build -x test
                    '''
                    
                }
                    
            }
        }
        stage('DockerSize'){
            steps {
	    	script {
		
                sh '''
                    docker stop gateway || true
                    docker rm gateway || true
                    docker rmi gateway-img || true
                    docker build -t gateway-img:latest .
                '''
		}

            }
        }
        stage('Deploy'){
            steps{
                sh 'docker run --network spharos-network --restart=always -e EUREKA_URL="${EUREKA_URL}" -d --name gateway -p 8000:8000 gateway-img'

            }
        }

    }
}

