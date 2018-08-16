pipeline {
   agent {label "${target_server}"}
      stages {
         stage('Import Docker-Build Images') {
            steps {
             sh '''#!/bin/bash -xe
             if test ! -z "$(docker images -q yi/tflow-build:0.6-python-v.3.6.3)"; then
                echo "Docker Image Already Exist!!!"
             else
                pv /media/common/DOCKER_IMAGES/TFlow-Build/yi-tflow-build-ssh-0.6-python-v.3.6.3.tar | docker load
                docker tag 5115755d4d48 yi/tflow-build:0.6-python-v.3.6.3
                echo "DONE!!!"
             fi
             if test ! -z "$(docker images -q yi/tflow-build:0.6)"; then
                echo "Docker Image Already Exist!!!"
                  else
                pv /media/common/DOCKER_IMAGES/TFlow-Build/yi-tflow-build-ssh-0.6.tar | docker load
                docker tag 0a1860a9598e yi/tflow-build:0.6
                echo "DONE!!!"
             fi
                '''
            }
    }
	      stage('Test yi/tflow-build:0.6-python-v.3.6.3 Docker Image') { 
            steps {
                sh '''#!/bin/bash -xe
		              echo 'Hello, BUILD-DOCKER!!'
                    image_id="$(docker images -q yi/tflow-build:0.6-python-v.3.6.3)"
                      if [[ "$(docker images -q yi/tflow-build:0.6-python-v.3.6.3 2> /dev/null)" == "$image_id" ]]; then
                          docker inspect --format='{{range $p, $conf := .RootFS.Layers}} {{$p}} {{end}}' $image_id
                      else
                          echo "It appears that current docker image corrapted!!!"
                          exit 1
                      fi 
                   ''' 
            }
        }
		  stage('Test yi/tflow-build:0.6 Docker Image') { 
            steps {
                sh '''#!/bin/bash -xe
		              echo 'Hello, BUILD-DOCKER!!'
                    image_id="$(docker images -q yi/tflow-build:0.6)"
                      if [[ "$(docker images -q yi/tflow-build:0.6 2> /dev/null)" == "$image_id" ]]; then
                          docker inspect --format='{{range $p, $conf := .RootFS.Layers}} {{$p}} {{end}}' $image_id
                      else
                          echo "It appears that current docker image corrapted!!!"
                          exit 1
                      fi 
                   ''' 
            }
        }
    }
	post {
            always {
               script {
                  if (currentBuild.result == null) {
                     currentBuild.result = 'SUCCESS' 
                  }
               }
               step([$class: 'Mailer',
                     notifyEveryUnstableBuild: true,
                     recipients: "igor.rabkin@xiaoyi.com",
                     sendToIndividuals: true])
            }
         } 
}
