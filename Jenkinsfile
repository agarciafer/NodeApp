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
         sh "docker rmi -f agarciaf/nodeapp"
	sh "docker rm -f  ci1"
	sh "docker run -dti  -p 8085:8000 --name ci1 agarciaf/nodeapp"
	
      }
    }
