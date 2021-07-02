pipeline{
    //Directives
    agent any
    tools {
        maven 'maven'
    } 
    // After Installing pipeline utility in jenkins
    environment{
       ArtifactId = readMavenPom().getArtifactId()
       Version = readMavenPom().getVersion()
       Name = readMavenPom().getName()
       GroupId = readMavenPom().getGroupId()
    } 
    stages {
        // Specify various stage with in stages

        // stage 1. Build
        /* stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        } */

        stage ('Sonarqube Analysis'){
            steps {
                echo ' Source Code Published in Sonarqube for Static Code Analysis'
                withSonarQubeEnv('sonarqube') {// You can override the credential to be used
                    sh 'mvn clean package sonar:sonar'
    }

            }
        }
      // Stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus'){
            steps {
                script {

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "demoapp-SNAPSHOT" : "demoapp-RELEASE"
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}",
                classifier: '',
                file: "target/${ArtifactId}-${Version}.war",
                credentialsId: '25e1c680-a654-445b-9675-f034d25c10dc',
                groupId: "${GroupId}",
                nexusUrl: 'localhost:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: "${NexusRepo}",
                version: "${Version}"
                }
            }
        } 

        // Stage 4: Print some information
      
       stage ('Print Environment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
                    }
                }

        // Stage 4 : Deploying the build artifact to Apache Tomcat
    /*    stage ('Deploy to Tomcat'){
            steps {
                echo "Deploying ...."
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_as_tomcat_user.yaml -i /opt/playbooks/hosts',
                                execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        }

    // Stage 6 : Deploying the build artifact to Docker
        stage ('Deploy to Docker'){
            steps {
                echo "Deploying ...."
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible_Controller', 
                    transfers: [
                        sshTransfer(
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yaml -i /opt/playbooks/hosts',
                                execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            
            }
        } */




    }

}
