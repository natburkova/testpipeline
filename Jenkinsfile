#!groovy
try {
node ('master') {

   stage 'Stage 1 - echo'
   echo 'Stage 1 started'
   
/* pause until you approve to go further without job abortion
   input 'Please decide if if should go further?'
   another job is called
   stage 'Stage 3'
   build job: 'hello-task', parameters: [[$class: 'StringParameterValue', name: 'CoolParam', value: 'hello']]
*/  

   stage 'Stage 2 - test timeout'
//   pause and job abortion if not approved
   timeout(time:4, unit:'MINUTES') {
   input message:'Approve deployment?'

}

   stage 'Stage 3 - echo'
   echo 'Stage 3 started'
 
   stage 'Stage 4  - Parallel checkout execution: Checkout and Installation on master and slave'
   parallel (
   master: { node ('master') {
   git 'https://github.com/natburkova/game-of-life'
   withMaven {sh 'mvn clean install'}
   
   }}, 
   slave: { node ('Ubuntu_vagrant'){
   git 'https://github.com/natburkova/game-of-life'
   sh 'cat pom.xml' 
   }}
   )
   stage 'Stage 5 - echo'
   echo 'Stage 5 started'
   
   
   }
   }
// catches FlowInterruptedException in case pipeline frow is aborted due to the absence of reaction in Stage 4
 catch (org.jenkinsci.plugins.workflow.steps.FlowInterruptedException e) {
    currentBuild.result = 'SUCCESS'
    echo 'Ignoring abort attempt at this spot'
} catch (InterruptedException x) {
    currentBuild.result = 'ABORTED'
    echo 'Ignoring abort attempt at this spot'
}

finally  
{ node('master') {
        sh "echo THE END"
}
}