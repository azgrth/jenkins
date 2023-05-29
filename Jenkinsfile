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
    if [ ! ${repo_url} ]; then exit 1; fi
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
    stage("deploy") {
      def ivu-ritpom-app1 = [:]
      ivu-ritpom-app1.name = "ivu-ritpom-app1"
      ivu-ritpom-app1.host = 192.168.10.221
      ivu-ritpom-app1.port = 22
      ivu-ritpom-app1.allowAnyHosts = true

      def ivu-ritpom-app2 = [:]
      ivu-ritpom-app2.name = "ivu-ritpom-app2"
      ivu-ritpom-app2.host = 192.168.10.222
      ivu-ritpom-app2.port = 22
      ivu-ritpom-app2.allowAnyHosts = true

      def ivu-ritpom-app3 = [:]
      ivu-ritpom-app3.name = "ivu-ritpom-app3"
      ivu-ritpom-app3.host = 192.168.10.223
      ivu-ritpom-app3.port = 22
      ivu-ritpom-app3.allowAnyHosts = true      

      cd $UUID
      withCredentials([sshUserPrivateKey(credentialsId: '4cabd9ec-56a3-4da3-a644-cb677972401c', passwordVariable: 'password', usernameVariable: 'userName')]) {
        ivu-ritpom-app1.user = userName
        ivu-ritpom-app1.password = password

        ivu-ritpom-app2.user = userName
        ivu-ritpom-app2.password = password

        ivu-ritpom-app3.user = userName
        ivu-ritpom-app3.password = password

        sshCommand remote: ivu-ritpom-app1, command: "if [ -d /var/lib/www/rockthecode/${UUID} ]; then mkdir -p; fi"        
        sshPut remote: ivu-ritpom-app1, from: 'build', into: '/var/lib/www/rockthecode/${UUID}'

        sshCommand remote: ivu-ritpom-app2, command: "if [ -d /var/lib/www/rockthecode/${UUID} ]; then mkdir -p; fi"        
        sshPut remote: ivu-ritpom-app2, from: 'build', into: '/var/lib/www/rockthecode/${UUID}'

        sshCommand remote: ivu-ritpom-app3, command: "if [ -d /var/lib/www/rockthecode/${UUID} ]; then mkdir -p; fi"        
        sshPut remote: ivu-ritpom-app3, from: 'build', into: '/var/lib/www/rockthecode/${UUID}'
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