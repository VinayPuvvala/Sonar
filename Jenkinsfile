pipeline {
    agent any
    
    stages {
        stage('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/VinayPuvvala/demo.git']]])
        }
        }
        /*stage('build') {
            steps {
                    withMaven(jdk: 'Java', maven: 'Maven') {
                      sh 'mvn clean compile'
                    }     
                  }
            }
        
        
        stage('Jacoco') {
            steps {
                jacoco()
           }
       }
        stage('Package') {
            steps {
                      withMaven(jdk: 'Java', maven: 'Maven') {
                        sh 'mvn package'
                    }
                }
        }
        /*stage('Build Docker Image') {
            
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo $curl(18.206.96.209:9000)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            
            steps {
                script {
                    docker.withRegistry('https://hub.docker.com/r/vpuvvala/demo/', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
         */   
        stage('DeployToProduction') {
            
            steps {
                /*input 'Deploy to Production?'
               milestone(1)
                withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: 'aws',
                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
            ]]) {
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'sample.yml',
                    enableConfigSubstitution: true
                )
                }*/
                sh '''
                export PATH=$PATH:/var/lib/jenkins
                kubectl get nodes
                kubectl create -f sample.yml
                kubectl get svc -w
                PID=$!
                sleep 5
                kill $PID
                '''
             }
            
        }
    }
}
