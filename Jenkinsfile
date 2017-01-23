#!groovy
node ('master') {
   stage 'Stage 1 - echo'
   echo 'Hello World 1'
   
// pause until you approve to go further without job abortion
   input 'Please decide if if should go further?'
// another job is called
   stage 'Stage 3'
   build job: 'hello-task', parameters: [[$class: 'StringParameterValue', name: 'CoolParam', value: 'hello']]
   
//   stage 'Stage 4 - test sleep'
// pause and job abortion if not approved
//   timeout(time:5, unit:'MINUTES') {
//   input message:'Approve deployment?'

//}
}  
   stage 'Parallel checkout execution: Checkout and Installation on master and slave'
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
   }
