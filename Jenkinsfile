pipeline {
   agent {label "master"}
    
    tools {
        maven 'maven'
        jdk '1.8'
       
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

       
      
      stage ('Compile-Package') {
            steps {
                git "https://github.com/quratulainleghari/my-app.git"
               sh 'mvn -f /var/lib/jenkins/workspace/sonar-pipeline/my-app-master package'
             // sh 'mvn package'
            }
          //post {
                //success {
                   // archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
                   // junit '**/target/surefire-reports/*.xml' 
               // }
           // }
      }
  
     
     stage ('Sonar-Analysis') {
        environment {
           
        def scannerhome = tool 'sonar'
        }
     steps {
   withSonarQubeEnv ('sonar-runner') {

 sh "${sonar-runner}/bin/sonar-runner -Dsonar.projectKey=my-app -Dsonar.projectName=my-app -Dsonar.projectVersion=1.0-Dsonar.sources=/var/lib/jenkins/workspace/$JOB_NAME/my-app-master/src"
    }

}
     }  
       
   stage('Deploy to Tomcat'){
  steps {
  sshagent(['3d0ff4fe-87e0-468b-9c6f-fbd6f291a57b']) {
    sh "scp  /var/lib/jenkins/workspace/sonar-pipeline/SimpleCustomerApp/target/*.war ubuntu@35.175.116.69:/opt/tomcat/apache-tomcat-8.5.37/webapps"
    
    }
    }
    }
  
       stage ('Email Notification'){
          steps{
      emailext (
    subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER}'",
    body: """<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>""",      
    to: "leghari.quratulain@gmail.com",
    from: "leghari.quratulain@gmail.com"
)
          }
       }
      }
   }


