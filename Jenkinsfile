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
    checkout([
        $class: 'GitSCM',
        branches: [[name: '*/${branch_name}']],
        doGenerateSubmoduleConfigurations: false,
        extensions: [],
        submoduleCfg: [],
        userRemoteConfigs: [[url: "${gitUrl}"]]
    ])  
    sh '''
    echo Variables from shell:
    printenv
    ls -la
    '''
 }
}
