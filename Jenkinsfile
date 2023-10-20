pipeline {
    agent any
    tools{
        maven 'myMaven'
    }

    stages {
        stage('Checkout the Source Code') {
            steps {
                echo 'Checkout the code..........................................'
                git 'https://github.com/sajidshaikhguru/addressbook-demo.git'
            }
        }
        stage('Build The Source Code') {
            steps{
			    script{
				     try{
                       echo 'Build the Source Code ....................................'
                       sh "mvn clean package"
					 }
					 catch(Exception e){
                         echo 'handling the exception ...................................'
                         emailext body: '''Hello Developer,

                         The Job got failed during Build.

                         Thanks,
                         Devops''', subject: 'Attention: $(JOB_NAME) is failed. Please look into the Build Number $(BUILD_NUMBER)', to: 'niladrimondal.mondal@gmail.com'
                    }
				
				}
                
            }
        } 
	 stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("sajidshaikhguru/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
       
    }
}
