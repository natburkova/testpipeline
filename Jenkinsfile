#!groovy
try {
 node('master') {
    
    env.WORKSPACE = pwd()
    env.TAG = "some_text_${currentBuild.number}"  
  stage('Checkout') { 
      checkout([$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'PerBuildTag'], [$class: 'CleanCheckout'], [$class: 'UserIdentity', email: 'natalia_burkova@epam.com', name: 'natalia.burkova.epam']], gitTool: 'Default', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/natburkova/hello-world.git']]])
     currentBuild.displayName = "#${currentBuild.number}"
     
   }
  
  
  node('master') {
    
    env.WORKSPACE = pwd()
    env.TAG = "some_text_${currentBuild.number}"  
    sh "git tag -a -m <messssss>"
    sh "git push --follow-tags --verbose "
   }
  
  
   
}

}catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
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
