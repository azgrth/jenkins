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

 stage("build") {
    def gitUrl = "https://github.com/azgrth/jenkins.git"
    def branch_name = "main"  
    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/azgrth/jenkins.git']])
    sh '''
    echo Variables from shell:
    printenv
    ls -la
    repo_url=`grep ${UUID} user-mappings.txt |awk '{print $2}'`
    echo ${repo_url}
    rm -rf ${UUID}
    mkdir ${UUID}
    cd $UUID
    git clone ${repo_url} .
    ls -la 
    npm test
    npm run build
    ls -la 
    '''
 }
}
