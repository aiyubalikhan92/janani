pipeline {
    agent any
    environment {
       registry = "123456889088.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER"
    }
    
   // cloning repository 
   
     stages {
        stage('Cloning Git') {
            steps {
            git branch: 'your-branch',  url: 'your_url'
     }
        }
 
  
  stage('Docker Login initial') {
      steps {
        sh '''
           set +x
          
           aws ecr get-login-password --region ap-southeast-1 --profile terraformadmin | docker login --username AWS --password-stdin 123456889088.dkr.ecr.ap-southeast-1.amazonaws.com

        '''
      }
    }
    
  
        // Building Docker images
stage('Building image'){
      steps {
        sh '''

               docker build --no-cache -t 123456889088.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER .

        '''
      }
    }
  
    //  Docker Push AWS ECR
    
     stage('Docker Push'){
      steps {
        sh '''
           set +x
           docker push 123456889088.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER
        '''
      }
    }
    
    // Docker Clean 
    
   stage('Docker Clean'){
      steps {
       sh '''
           set +x
           docker rmi 123456889088.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER
       '''
     }
    }



  stage('Docker updating') {
      steps{
        script {
          sh '''
            set +x    
            eb --version
            eb init --region ap-southeast-1 --platform Docker Jen-Git-Dkr-zips3-EB --profile terraformadmin
            eb use Jengitdkrzips3eb-env --region ap-southeast-1 --profile terraformadmin
            echo "updating"
            eb deploy Jengitdkrzips3eb-env --region ap-southeast-1 --profile terraformadmin
            echo "over"
            '''
           }
       }    
      }

      
    }  
}

