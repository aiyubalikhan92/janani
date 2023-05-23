pipeline {
    agent any
    environment {
       registry = "087019994649.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER"
    }
    
   // cloning repository 
   
     stages {
        stage('Cloning Git') {
            steps {
            git branch: 'jen-git-dkr-iEcr-eb',  url: 'git@bitbucket.org:aiyubalikhan92/docker-ecr.git'
            sh 'ls'
            sh 'pwd'
     }
        }
 
  
  stage('Docker Login initial') {
      steps {
        sh '''
           set +x
          
           aws ecr get-login-password --region ap-southeast-1 --profile terraformadmin | docker login --username AWS --password-stdin 087019994649.dkr.ecr.ap-southeast-1.amazonaws.com
           echo "login success"
        '''
      }
    }
    
  
        // Building Docker images
stage('Building image'){
      steps {
        sh '''
             # echo "$registry"
               docker build --no-cache -t 087019994649.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER .
             echo "done"
        '''
      }
    }
  
    //  Docker Push AWS ECR
    
     stage('Docker Push'){
      steps {
        sh '''
           set +x
           docker push 087019994649.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER
        '''
      }
    }
    
    // Docker Clean 
    
   stage('Docker Clean'){
      steps {
       sh '''
           set +x
           docker rmi 087019994649.dkr.ecr.ap-southeast-1.amazonaws.com/testrepo:$BUILD_NUMBER
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
      
      stage('Docker LIST'){
      steps {
        sh '''
           set +x
          
           echo "hippo---------------docker image ls---------------------------------------hippo"
           docker image ls   
           echo "hippo---------------------------------------END--------------------------------hippo"
           
           echo "hippo--------------------------------List-ECR-IMAGES-------------------------hippo"
           aws ecr list-images --repository-name testrepo --region ap-southeast-1 --profile terraformadmin
            echo "hippo------------------------List-ECR-IMAGES--END---------------------------------hippo"
           
           echo "hippo-------------------------------------------delete last ecr image --------------------hippo"
            aws ecr batch-delete-image --repository-name testrepo --image-ids imageTag=$BUILD_NUMBER  --region ap-southeast-1 --profile terraformadmin
            echo "hippo----------------------------------------delete last ecr image --END-------------hippo"
           
            echo "hippo----------------------------docker ps---------hippo"
            docker ps
            echo "hippo---------------------------------------docker ps----END-------------hippo"

            echo "hippo------------------------------------- docker container ls --END-------------hippo"
            docker container ls
            echo "hippo--------------------------------------- docker container ls --END-------------hippo"
        '''
      }
    }
    
    
      
    }  
}

