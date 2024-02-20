pipeline {
    
    tools {
  maven 'Maven3'
}

    agent any
    
    parameters {
    // choice(name: 'Branch', choices: 'create\nrollback', description: 'Create/rollback of the deployment')  //
	// choice(name: 'action', choices: 'create\nrollback', description: 'Create/rollback of the deployment') //
	string(name: 'Branch', description: "Select branch to build",defaultValue: "master")
    string(name: 'ImageName', description: "Name of the docker build", defaultValue: "sathishimage-alpha")
	string(name: 'ImageTag', description: "Name of the docker build",defaultValue: "v1")
	string(name: 'AppName', description: "Name of the Application",defaultValue: "SathishApp-alpha")
    string(name: 'docker_repo', description: "Name of docker repository",defaultValue: "sathsih")
  }
  
    stages {
      stage ('Checkout'){
        steps {
          git branch: '$Branch', credentialsId: 'github', url: 'https://github.com/sathishyadav4r/Jenkins_CICDrepo.git'
          
        }
        
      }
      
      }
  
}
