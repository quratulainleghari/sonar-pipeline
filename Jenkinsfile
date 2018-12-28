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
                git "https://github.com/javahometech/my-app"
              sh 'mvn clean package'
            }
          post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
                    junit '**/target/surefire-reports/*.xml' 
                }
            }
      }
  
     
     stage ('Sonar-Analysis') {
     steps {
 //defining sonar scanner home by giving scaner name or could be generated from pipeline syntax
   
 def scannerhome = tool 'sonar';
    
//using sonarqube environment by generating from pipeline syntax 
  
  withSonarQubeEnv ('sonar') {
   
 // using sonarqube code analysis via shell command

       
 sh """${sonar}/bin/sonar-runner -D sonar.login=admnin -D sonar.password=admin"""
    }
}
}
     
       
   stage('Deploy to Tomcat'){
  steps {
  sshagent(['3d0ff4fe-87e0-468b-9c6f-fbd6f291a57b']) {
    sh "scp  /var/lib/jenkins/workspace/tomcat-from-slave-to-app/target/*war ubuntu@35.175.116.69:/opt/tomcat/apache-tomcat-8.5.37/webapps"
    
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


