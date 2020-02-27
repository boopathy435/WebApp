
node {
    // Get Artifactory server instance, defined in the Artifactory Plugin administration page.
    def server = Artifactory.server "artifactory1"
    // Create an Artifactory Maven instance.
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo
    
 rtMaven.tool = "maven"

    stage('Clone sources') {
        git url: 'https://github.com/garima-sindhwani26/WebApp.git'
    }
    
     stage('Artifactory configuration') {
        // Tool name from Jenkins configuration
        rtMaven.tool = "maven"
        // Set Artifactory repositories for dependencies resolution and artifacts deployment.
        rtMaven.deployer releaseRepo:'repo1', snapshotRepo:'repo1', server: server
        //rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    }

    stage('Maven build') {
        buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean -DskipTests install'
    }
    
   stage('Build') {
   jiraSendBuildInfo branch: 'master', site: 'devopsbootcamp.atlassian.net'
} 
stage('functiontest')
{
blazeMeterTest credentialsId: 'blazemeter', testId: '7695651.taurus', workspaceId: '431777'
    stage('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
stage('slackpush')
{
slackSend channel: '#cloud', message: 'Build Started - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)'
}
    stage('deploy to tomcat'){
        deploy adapters: [tomcat7(credentialsId: 'tomcat-tharun', path: '', url: 'http://34.93.27.202:8080/')], contextPath: '/QAWebapp', onFailure: false, war: '**/*.war'
    
    }
	stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v1/', 'docker') {
       def app = docker.build("boopathy435/pheonix:${version}", '.').push()
       slackSend message: "docker image boopathy435/pheonix:${version} built and pushed to repo. ";
     }

    }

}
