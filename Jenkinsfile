pipeline{
    agentany
    tools{
            jdk 'jdk11'
            maven 'maven3'
    }
    stages{
        stage ('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage ('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/krishnar700/Project1.git']])
                }
        }
        stage ('build stage'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage ('sonarqube analysis'){
            steps{
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-server') {
                      sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage('SonarQube Quality Gates') {
            steps {
                script {
                    def qg = waitForQualityGate abortPipeline: false, credentialsId: 'sonar-server'
                    if (qg.status != 'OK') {
                        error "Quality Gate check failed: ${qg.status}"
                    }
                }
            }
        }
    }
}

