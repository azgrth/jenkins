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
      def ivu_ritpom_app1 = [:]
      ivu_ritpom_app1.name = "ivu_ritpom_app1"
      ivu_ritpom_app1.host = "192.168.10.221"
      ivu_ritpom_app1.port = 22
      ivu_ritpom_app1.allowAnyHosts = true

      def ivu_ritpom_app2 = [:]
      ivu_ritpom_app2.name = "ivu_ritpom_app2"
      ivu_ritpom_app2.host = "192.168.10.222"
      ivu_ritpom_app2.port = 22
      ivu_ritpom_app2.allowAnyHosts = true

      def ivu_ritpom_app3 = [:]
      ivu_ritpom_app3.name = "ivu_ritpom_app3"
      ivu_ritpom_app3.host = "192.168.10.223"
      ivu_ritpom_app3.port = 22
      ivu_ritpom_app3.allowAnyHosts = true      

      // cd $UUID
      withCredentials([usernamePassword(credentialsId: '4cabd9ec-56a3-4da3-a644-cb677972401c', passwordVariable: 'password', usernameVariable: 'userName')]) {
        ivu_ritpom_app1.user = userName
        ivu_ritpom_app1.password = password

        ivu_ritpom_app2.user = userName
        ivu_ritpom_app2.password = password

        ivu_ritpom_app3.user = userName
        ivu_ritpom_app3.password = password
        
        echo UUID
        echo env.UUID
        
        def file_source = UUID + '/build'
        def file_dest = "/var/lib/www/rockthecode/" + UUID
        // sshCommand remote: ivu_ritpom_app1, command: "if [ -d /var/lib/www/rockthecode/" + UUID + "]; then mkdir -p /var/lib/www/rockthecode/" + UUID + "; fi"
        // sshPut remote: ivu_ritpom_app1, from: file_source, into: file_dest

        // sshCommand remote: ivu_ritpom_app2, command: "if [ -d /var/lib/www/rockthecode/${UUID} ]; then mkdir -p; fi"        
        // sshPut remote: ivu_ritpom_app2, from: 'build', into: '/var/lib/www/rockthecode/${UUID}'

        // sshCommand remote: ivu_ritpom_app3, command: "if [ -d /var/lib/www/rockthecode/${UUID} ]; then mkdir -p; fi"        
        // sshPut remote: ivu_ritpom_app3, from: 'build', into: '/var/lib/www/rockthecode/${UUID}'
      }
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