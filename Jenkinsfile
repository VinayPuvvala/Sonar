pipeline {
    agent any
    environment {
        KUBECONFIG = credentials('kubeconfig')
        AWS = credentials('aws')
    }
    stages {
        stage('build') {
            steps {
                    withMaven(jdk: 'Java', maven: 'Maven') {
                      sh 'mvn clean compile'
                    }     
                  }
            }
        
        
        /*stage('Jacoco') {
            steps {
                jacoco()
           }
       }*/
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
                input 'Deploy to Production?'
               milestone(1)
                withCredentials(credentials:'$AWS') {
                kubernetesDeploy(
                    kubeconfigId: '$KUBECONFIG',
                    configs: 'sample.yml',
                    enableConfigSubstitution: true
                )
                }
            }
                
        }
    }
}
