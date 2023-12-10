pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage('Checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Tejas-Pulli/MavenDemo.git']])
            }
        }
        stage('Build'){
            steps{
                // sh 'mvn clean install -f **/pom.xml'
                build quietPeriod: 2, job: 'MyCDPipeline'
            }
        }
     
        stage('Dev Deploy'){
            steps{
              deploy adapters: [tomcat9(credentialsId: 'dd73c879-7056-4ff4-9d17-fa1e9f558677', path: '', url: 'http://localhost:8081/manager/text')], contextPath: 'SampleMaven', war: '**/*.war'
              }
        }
        
    }
}
