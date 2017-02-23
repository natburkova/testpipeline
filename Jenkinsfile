#!groovy
//try {
 node('master') {
     stage('Checkout') { 
    env.WORKSPACE = pwd()
    env.TAG = "some_text_${currentBuild.number}"
    env.REPO_URL="github.com/natburkova/hello-world.git"
 
      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://github.com/natburkova/hello-world.git']]])
     currentBuild.displayName = "#${currentBuild.number}"
     
   }
  
  
  
    stage('tag') { 
    env.WORKSPACE = pwd()
    //env.TAG = "${currentBuild.number}"
  
     
     withCredentials([usernamePassword(credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
     sh '''
git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@${REPO_URL}
git checkout master
      
      
      TAG=${targetRepo}_${targetBranch}.${BUILD_NUMBER}
      git tag -l ${TAG}
      git tag -a -m "Tag has been made by CI" ${TAG}
      #git config --local push.default simple
      #git push --set-upstream origin master --follow-tags --verbose
     '''
    }
   }
  
  
  
   
}

