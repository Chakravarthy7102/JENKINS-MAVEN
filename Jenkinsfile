pipeline{
    agent any
    tools{
        maven 'Maven3'
    }
    
    stages{
        stage('Checkout'){
            steps{
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Gouthama1184/Jenkins_Sample_Java_Project.git']]])
            }   
        }
        stage('Build'){
            steps{
                sh 'mvn clean install -f Jenkins_Sample_Java_Project/pom.xml'
            }
        }
        
        stage('Code Quality'){
            steps{
                withSonarQubeEnv('SonarQube')
                sh 'mvn sonar:sonar -f Jenkins_Sample_Java_Project/pom.xml'
            }
        }
        
        stage('Nexus Uplaod'){
            steps{
                 nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: 'nexus_url:8081',
                  groupId: 'myGroupId',
                  version: '1.0-SNAPSHOT',
                  repository: 'maven-snapshots',
                  credentialsId: 'fc0f1694-3036-41fe-b3e3-4c5d96fcfd26',
                  artifacts: [
                  [artifactId: 'Jenkins_Sample_Java_Project',
                  classifier: '',
                  file: 'Jenkins_Sample_Java_Project/target/Jenkins_Sample_Java_Project.war',
                  type: 'war']
                  ])
            }
        }
        
        stage ('DEV Deploy') {
         steps {
              echo "deploying to DEV Env "
              deploy adapters: [tomcat9(credentialsId: '268c42f6-f2f5-488f-b2aa-f2374d229b2e', path: '', url: 'http://your_public_dns:8080')], contextPath: null, war: '**/*.war'
            }
        }
        
    }
    
}