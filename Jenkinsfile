node {


    
    def app
    def dockerRegistry
    def dockerCreds
    
    parameters {
        string(name: 'Branch', defaultValue: 'master', description: 'Git Commit Hash')
    }


    stage('Clone repository') {
        /* Let's make sure we h ave the repository cloned to our workspace */

        /*checkout scm*/
        checkout ([
            $class: 'GitSCM',
            branches: [[name: '*/master']],
            userRemoteConfigs: [[
            url: 'https://github.com/venky2291/Mainstay.git']]
                   ])
    }

    stage('Build image') {

           echo 'Starting to build docker image'

	   dockerRegistry = 'https://registry.hub.docker.com'
           dockerCreds = 'docker-venky2291-artifactory'

	   docker.withRegistry(dockerRegistry, dockerCreds) {
 
                  def customImage = docker.build("venky2291/mainstay:latest-${env.BUILD_NUMBER}")

	          echo 'Pushing the Image to Registry'
                  
	          customImage.push()
	 }	  

    }

 
    stage('Create Properties file') {
        sh "docker inspect --format=\'{{index .RepoDigests 0}}\'  https://registry.hub.docker.com/venky2291/mainstay:${env.BUILD_NUMBER}>image.properties"
        
        
      
        archiveArtifacts artifacts: 'image.properties'
    }
        
}
