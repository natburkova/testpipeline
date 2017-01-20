#!groovy
node ('master') {
   stage 'Stage 1'
   echo 'Hello World 1'
   input 'Please decide if if should go further?'

   stage 'Stage 3'
   build job: 'hello-task', parameters: [[$class: 'StringParameterValue', name: 'CoolParam', value: 'hello']]
   
   stage 'Stage 4'
   sh 'sleep 10'
   
   stage 'Stage 5 - Checkout'
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://github.com/natburkova/game-of-life']]])
   
   
   stage 'Stage 6 - Installation'
   withMaven {sh 'mvn clean install'}
 }  
  
   stage 'Parallel execution: Checkout and Installation on master and slave'
   parallel (
   master: { node ('master') {
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://github.com/natburkova/game-of-life']]])
   withMaven {sh 'mvn clean install'}
   }}, 
   slave: { node ('Ubuntu_vagrant'){
   checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0ab90352-3a22-4f26-abc0-74f368677e3a', url: 'https://github.com/natburkova/game-of-life']]])
   withMaven {sh 'mvn clean install'}
   }}
   
   )