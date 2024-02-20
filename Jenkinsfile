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
      
      stage('SonarQube Analysis') {
        def mvnHome =  tool name: 'maven-3', type: 'maven'
        withSonarQubeEnv('sonar-6') { 
          //sh "${mvnHome}/bin/mvn sonar:sonar"//
	  sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
        }
    }
    
    stage("Quality Gate Statuc Check"){
          timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                   slackSend baseUrl: 'https://hooks.slack.com/services/',
                   channel: '#jenkins-pipeline-demo',
                   color: 'danger', 
                   message: 'SonarQube Analysis Failed', 
                   teamDomain: 'javahomecloud',
                   tokenCredentialId: 'slack-demo'
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }

     stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version} ARTVERSION";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: ARTVERSION,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } 
		    else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        } 
  }
  
}
