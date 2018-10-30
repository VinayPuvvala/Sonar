pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                      sh 'mvn clean compile' 
                       
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
            
        stage('DeployToProduction') {
            
            steps {
                input 'Deploy to Production?'
               milestone(1)
                kubernetesDeploy(
                    kubeconfigId: 'kubeconfig',
                    configs: 'sample.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
