node {
 properties([
  pipelineTriggers([
   [$class: 'GenericTrigger',
    genericRequestVariables: [
     [key: 'UUID', regexpFilter: '']
    ],

    causeString: 'Triggered on $ref',

    token: '8b7f4637-3990-4f04-87f1-99c41f61a7f8',
    tokenCredentialId: '',

    printContributedVariables: true,
    printPostContent: true,

    silentResponse: false,
    
    shouldNotFlattern: false
   ]
  ])
 ])

  stage("prepare") {
    def gitUrl = "https://github.com/azgrth/jenkins.git"
    def branch_name = "main"  
    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/azgrth/jenkins.git']])
    sh '''
    repo_url=`grep ${UUID} user-mappings.txt |awk '{print $2}'`
    rm -rf ${UUID}
    mkdir ${UUID}
    cd $UUID
    git clone ${repo_url} .
    npm install
    '''
  }
  try {

    stage("build") {
      sh '''
      cd $UUID
      npm run build
      '''
    }
  } catch (e) {
        // If there was an exception thrown, the build failed
        echo 'Exception occurred: ' + e.toString()
        currentBuild.result = "FAILED"
        throw e
    } finally {
        // Success or failure, always send notifications
        // notifyBuild(currentBuild.result)
    }
}