#!groovy
node ('master') {
   stage 'Stage 1 - echo'
   echo 'Hello World 1'
   
#pause until you approve to go further without job abortion
   input 'Please decide if if should go further?'
#another job is called
   stage 'Stage 3'
   build job: 'hello-task', parameters: [[$class: 'StringParameterValue', name: 'CoolParam', value: 'hello']]
   
   stage 'Stage 4 - test sleep'
#pause and job abortion if not approved
   timeout(time:5, unit:'MINUTES') {
   input message:'Approve deployment?'

}
}  
  
   stage 'Parallel execution: Checkout and Installation on master and slave'
   parallel (
   master: { node ('master') {
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://github.com/natburkova/game-of-life']]])
#   stash name: 'sources', includes: 'pom.xml,src/'
   withMaven {sh 'mvn clean install'}
   }}, 
   slave: { node ('Ubuntu_vagrant'){
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://github.com/natburkova/game-of-life']]])
   withMaven {sh 'mvn clean install'}
   }}
   
   )