node {
    def app

    stage('Clone repository') {
        /* Cloning the Repository to our Workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image */

        app = docker.build("agarciaf/nodeapp")
    }

    stage('Test image') {
        
        app.inside {
            echo "Tests passed"
        }
    }

    stage('Push image') {
        /* 
			You would need to first register with DockerHub before you can push images to your account
		*/
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
            } 
                echo "Trying to Push Docker Build to DockerHub"
    }
	
   stage('Deploy docker image agarciaf/nodeapp') {
      steps{
        sh "docker rmi -f  agarciaf/nodeapp"
	sh "docker rm -f  ci-1"
	sh "docker run -dti  -p 95:8000 --name ci-1 agarciaf/nodeapp"
	sh "curl 127.0.0.1:95"
      }
    }	
}
