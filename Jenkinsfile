pipeline {

  agent any
  
  parameters {
	choice(name: 'action', choices: 'create\nrollback', description: 'Create/rollback of the deployment')
    string(name: 'ImageName', description: "Name of the docker build", defaultValue: "sathishApp")
	string(name: 'ImageTag', description: "Name of the docker build",defaultValue: "v1")
	string(name: 'AppName', description: "Name of the Application",defaultValue: "SathishApp")
    string(name: 'docker_repo', description: "Name of docker repository",defaultValue: "sathsih4r")
    string(name: 'BRANCH', description: "Select the branch to build",defaultValue: "master")
  }
  
  tools{ 
        maven 'Maven3'
    }
    stages {
        stage('Git-Checkout') {
            steps {
            git branch: '$BRANCH', credentialsId: 'github', url: 'https://github.com/sathishyadav4r/Jenkins_CICDrepo.git'
        
            }
        }
        
    }
}
  
