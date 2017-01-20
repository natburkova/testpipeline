#!groovy
node {
   stage 'Stage 1'
   echo 'Hello World 1'
   input 'Please decide if if should go further?'

   stage 'Stage 2'
   echo 'Hello World 2'

   stage 'Stage 3'
   echo 'Hello World 3'

   stage 'Stage 4'
   build job: 'hello-task', parameters: [[$class: 'StringParameterValue', name: 'CoolParam', value: 'hello']]
   
   stage 'Stage 5'
   sh 'sleep 10'
   
   stage 'Stage 7 - Checkout'
   git 'https://github.com/natburkova/game-of-life'
   
   
   stage 'Stage 8 - echo task'
   echo "hello there!"
   
   stage 'Stage 9 - Installation'
   withMaven {sh 'mvn clean install'}
 }  
 node ('Ubuntu_vagrant') {
   stage 'Stage 1 on Ubuntu - Checkout'
   git 'https://github.com/natburkova/game-of-life'
 
   stage 'Stage 2 on Ubuntu - Installation'
   withMaven {sh 'mvn clean install'}
 }
   
   stage 'Parallel test'
   parallel (
   first_branch: { node {
   echo "hello master!"
   }}, 
   second_branch: { node ('Ubuntu_vagrant'){
   echo "hello Ubuntu!"
   }})
