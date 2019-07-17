pipeline{
    agent any
       environment {
        
         FLASK_APP='hello_flask.py'
         FLASK_ENV='production'
 
       }
    stages{
      
        
        
     stage ('Installing dependencies'){
            steps{
              
                    sh 'pip install -r requirements.txt'
               
            }
        }
           
        stage ('Testing'){
            steps{
         
        sh 'coverage run tests.py'
            }
        }
        
          stage ('Sonar Code Analysis'){
              environment {
                 scannerHome=tool 'sonar scanner'
            }
              steps{
             sh 'coverage html tests.py'

           sh '${scannerHome}/bin/sonar-scanner -Dproject.settings=./sonar-project.properties'
         
              }
          }
          stage ('zipping files')
        {
            steps
            {
                 
                sh 'zip -r neha.zip .'
            }
        }
        stage ( 'Uploading to nexus')
        {
            steps{
                withCredentials([usernamePassword(credentialsId: 'sudipa_nexus', passwordVariable: 'pass', usernameVariable: 'usr')]){
                sh label: '', script: 'curl -u ${usr}:${pass} --upload-file neha.zip http://3.14.251.87:8081/nexus/content/repositories/devopstraining/Neha-python/neha.zip'
            }
            
        }
        }
            
          
        stage ('Deployment'){
      steps{
        sh "JENKINS_NODE_COOKIE=dontKillMe forever start -c python flaskblog.py -a -uid mypyapp"
         
        }
    }
     
         
        
        
    }
}
    
    
    
