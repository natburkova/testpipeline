#!groovy
try {
 node('master') {
    
    env.WORKSPACE = pwd()
      
  stage('Checkout') { 
      checkout([$class: 'GitSCM', branches: [[name: '${branch}']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'PerBuildTag'], [$class: 'CleanCheckout'], [$class: 'UserIdentity', email: 'natalia_burkova@epam.com', name: 'natalia.burkova.epam']], gitTool: 'Default', submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git', url: 'https://github.com/natburkova/game-of-life']]])
     currentBuild.displayName = "#${currentBuild.number}"
    
   }
   
}

}catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
    echo "This job was cancelled or aborted"
    currentBuild.result = 'SUCCESS'
} catch (err) {
    node('master') {
    echo "Exception thrown:\n ${err}"
    currentBuild.result = 'FAILURE'
    emailext body: '''Job $JOB_NAME FAILED.\nPlease see $BUILD_URL/console for the error details''', subject: 'BUILD JOB FAILED: $JOB_NAME', to: 'natalia_burkova@epam.com,Yanina_Hulevich@epam.com'
} 
}finally {
    node('master') {
        sh "echo ${currentBuild.result}"
    }
}
