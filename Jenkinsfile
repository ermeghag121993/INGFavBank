pipeline {
agent any
stages {
        stage('Git checkout')
            {
            steps {
              git 'https://github.com/ermeghag121993/INGFavBank'
              }
            }
        stage('Build Analysis') 
            {
            steps {
                    withSonarQubeEnv('sonar')  
                  { 
                  sh '/opt/apache-maven-3.6.2/bin/mvn clean verify sonar:sonar -Dsonar.password=admin -Dsonar.login=admin' 
                  }
                } 
            } 
        stage('Quality Gate Check') 
         { 
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
                    }
                 } 
             }
        stage('Deploy') 
         { 
            steps {
             sh '/opt/apache-maven-3.6.2/bin/mvn clean deploy'
            }
         } 
        stage('Execute') 
         { 
            steps {
             sh 'export JENKINS_NODE_COOKIE=dontKillMe ;nohup java -jar $WORKSPACE/target/*.jar &' 
             }
         }
    }
}
