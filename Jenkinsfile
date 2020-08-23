node {
    git poll: true, url: 'https://github.com/ubiopen/docker-jenkins-workshop.git'
    
    withCredentials([[$class: 'UsernamePasswordMultiBinding', \
            credentialsId: 'dockerhub', \
            usernameVariable: 'DOCKER_USER_ID', \
            passwordVariable: 'DOCKER_USER_PASSWORD']]) {
	    stage('Pull') {
	        git 'https://github.com/ubiopen/docker-jenkins-workshop.git'
	    }
	    stage('Unit Test') {

	    }
	    stage('Build') {
	    	sh(script: '''docker build --force-rm=true \
				-t ${DOCKER_USER_ID}/docker_tutorial . ''')
	    }
	    stage('Tag') {
	        sh(script: '''docker tag ${DOCKER_USER_ID}/docker_tutorial \
	            ${DOCKER_USER_ID}/docker_tutorial:${BUILD_NUMBER}''')
	    }
	    stage('Push') {
	        sh(script: 'docker login -u ${DOCKER_USER_ID} -p ${DOCKER_USER_PASSWORD}')
	        sh(script: 'docker push ${DOCKER_USER_ID}/docker_tutorial:${BUILD_NUMBER}')
	        sh(script: 'docker push ${DOCKER_USER_ID}/docker_tutorial:latest')
	    }
	    stage('Deploy') {
	        try {
        		sh(script: 'docker stop docker_tutorial')
        		sh(script: 'docker rm docker_tutorial')
        	} catch(e) {
        		echo "No docker_tutorial container exists"
        	}
        	sh(script: '''docker run -d -p 10000:4567 --name=docker_tutorial \
        		${DOCKER_USER_ID}/docker_tutorial:${BUILD_NUMBER}''')
	    }
	}
}
