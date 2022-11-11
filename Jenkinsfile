pipeline{
    agent any
    	environment {
		notifyEmail ="varun.allawadhi@nagarro.com"
	}
    //tools{
        //maven 'automaven'
   // }
    stages{
        stage("code checkout"){
            steps{
            checkout scm
            }
        }   
        stage("code build"){
            steps{
            bat "mvn clean"
            }
        }
        stage("unit test"){
            steps{
            bat "mvn test"
            }
        }
        stage("Sonar Analysis"){
            steps{
             withSonarQubeEnv("test_sonar")
                 {
                 bat 'mvn sonar:sonar'
                 }
             }
        }
        stage("Publish to Artifactory"){
            steps{
                rtMavenDeployer(
                    id: 'deployer',
                    serverId: '123456789@artifactory',
                    releaseRepo: 'varun.nagp',
                    snapshotRepo: 'varun.nagp'
                )
                rtMavenRun(
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer'
                    )
                rtPublishBuildInfo(
                    serverId:'123456789@artifactory',
                )
            }        
        }
        
    }
    post{
        success{
            bat "echo success"
            }
        }
}
