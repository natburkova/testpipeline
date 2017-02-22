#!groovy
//try {
 node('master') {
     stage('Checkout') { 
    env.WORKSPACE = pwd()
    env.TAG = "some_text_${currentBuild.number}"
    env.REPO_URL="github.com/natburkova/hello-world.git"
 
      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://${REPO_URL}']]])
     currentBuild.displayName = "#${currentBuild.number}"
     
   }
  
  
  
    stage('tag') { 
    env.WORKSPACE = pwd()
    //env.TAG = "${currentBuild.number}"
  
     
     withCredentials([usernamePassword(credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
     sh '''
git remote set-url origin https://${GIT_USERNAME}:${GIT_PASSWORD}@REPO_URL
git checkout master
      
      
      TAG=${targetRepo}_${targetBranch}.${BUILD_NUMBER}
      git tag -l ${TAG}
      git tag -a -m "Tag has been made by CI" ${TAG}
      git config --local push.default simple
      git push --set-upstream origin master --follow-tags --verbose
     '''
    }
   }
  
  
  
   
}

/*}catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
    echo "This job was cancelled or aborted"
    currentBuild.result = 'SUCCESS'
} catch (err) {
    node('master') {
    echo "Exception thrown:\n ${err}"
    currentBuild.result = 'FAILURE'
    emailext body: '''Job $JOB_NAME FAILED.\nPlease see $BUILD_URL/console for the error details''', subject: 'BUILD JOB FAILED: $JOB_NAME', to: 'natalia_burkova@epam.com'
} 
}finally {
    node('master') {
        sh "echo ${currentBuild.result}"
    }
}
*/
