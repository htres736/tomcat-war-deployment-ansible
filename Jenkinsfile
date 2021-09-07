pipeline{
    agent any
    tools{
        maven 'maven'
        jdk 'JDK11'        
    }
    
    stages{
        stage('Code Compile'){
            steps{
                        
                sh """
                    echo "compile code"
                    mvn compile
                    echo "code compile complete"
                """
               
            }
        }
        stage ('Packaging'){
            steps{
                catchError (buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                sh """
                    echo "package code"
                    mvn clean package
                    echo "code packaging complete"
                """
                }
            }
        }
        stage ('Deploy'){
            steps{
                // Execute the ansible playbook to install tomcat, copy the .war file, and restart tomcat
                ansiblePlaybook credentialsId: 'JenkinsAnsible', disableHostKeyChecking: true, installation: 'ansible', inventory: 'AWS.inv', playbook: 'tomcatDeploy.yml'
            }
        }
    }
}
