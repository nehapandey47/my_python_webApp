pipeline{
    agent any
       environment {
        
         FLASK_APP='hello_flask.py'
         FLASK_ENV='production'
 
       }
    stages{
        stage('checkout'){
            steps{
               // git 'https://github.com/nehapandey47/my_python_webApp.git'
                  withCredentials([string(credentialsId: 'neha_app', variable: 'git')]) {
                  echo "My password is '${git}'!"
                  checkout([$class: 'GitSCM',
                  branches: [[name: 'origin/master']],
                  extensions: [[$class: 'WipeWorkspace']],
                  userRemoteConfigs: [[url: "${git}"]]
                 ])
            }
            }
        }
        
     stage ('dependencies'){
            steps{
              
                    sh 'pip install -r requirements.txt'
               
            }
        }
           
        stage ('Testing'){
            steps{
         // echo "test"
        sh 'coverage run tests.py'
            }
        }
        
          stage ('Analysis'){
              environment {
                 scannerHome=tool 'sonar scanner'
            }
              steps{
             sh 'coverage html tests.py'

           sh '${scannerHome}/bin/sonar-scanner -Dproject.settings=./sonar-project.properties'
         
              }
          }
          stage ('zip')
        {
            steps
            {
                 
                sh 'zip -r neha.zip .'
            }
        }
        stage ( 'nexus')
        {
            steps{
                withCredentials([usernamePassword(credentialsId: 'sudipa_nexus', passwordVariable: 'pass', usernameVariable: 'usr')]){
                sh label: '', script: 'curl -u ${usr}:${pass} --upload-file neha.zip http://3.14.251.87:8081/nexus/content/repositories/devopstraining/Neha-python/neha.zip'
            }
            
        }
        }
          
          
          
          
          
          
          
          
          stage ('Deploy'){
      steps{
        sh "JENKINS_NODE_COOKIE=dontKillMe forever start -c python flaskblog.py -a -uid mypyapp"
         
        }
    }
     
          
          
          
          
          
          
          
        
    //  stage ('Deploy'){
    //   steps{
    //      //   sh 'python flaskblog.py'
    //      sh 'whoami'
    //          sh '''nohup python flaskblog.py %26'''
    //      sh 'curl localhost:5000;ps ax | grep flaskblog.py'
         
    //          }
    //      }
        
        
    }
}
    
    
    
