pipeline{
    tools{
        jdk 'Java8'
        maven 'Maven_auto'
    }
    agent none
        stages{
             stage('Compile'){
                agent any
                steps{
				    git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
                    sh 'mvn compile'
                }
                
            }
            stage('CodeReview'){
                agent any
                steps{
				git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
                    sh 'mvn pmd:pmd'
                }
                post{
                    always{
                        pmd pattern: 'target/pmd.xml'
                    }
                }
            }
            stage('UnitTest'){
                agent any
                steps{
                    git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
                    sh 'mvn test'
                }
                
            }
            stage('MetriCheck'){
                agent any
                steps{
		git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
                    sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'
                }
                post{
                    always{
                        cobertura coberturaReportFile: 'target/site/cobertura/coverage.xml'
                    }
                }
            }
            stage('Package'){
                agent {label 'linux_slave' }
                steps{
		git 'https://github.com/devops-trainer/DevOpsClassCodes.git'
                    sh 'mvn package'
                }
		    
            }
            stage('Deploy'){ 
		    agent {label 'linux_slave' }
              steps{
              sh "rm -rf *"
              sh "ls -l"
              sh "cp /var/lib/jenkins/workspace/package/target/addressbook.war ."
	      git 'https://github.com/nkchand91/docker.git'			
              sh "docker build -t myimages:$BUILD_NUMBER ."
              //cant give a fix tag coz every time new image will created, we will use Env_var
               sh "docker images"
               sh "docker run -itd -P myimages:$BUILD_NUMBER"
               //we are not giving any container name as we are depoying new image.
               sh "docker ps"
               //to show that container is running.
  
                }
            }
    
    }
}
