pipeline {
   agent {label "${target_server}"}
      stages {
         stage('Import Docker-Build Images') {
            steps {
             sh '''#!/bin/bash -xe
             if test ! -z "$(docker images -q yi/tflow-build:0.7-python-v.3.6.3)"; then
                echo "Docker Image Already Exist!!!"
             else
                pv -f /media/common/DOCKER_IMAGES/TFlow-Build/yi-tflow-build-ssh-0.7-python-v.3.6.3.tar | docker load
                docker tag 14ba469650ba yi/tflow-build:0.7-python-v.3.6.3
                echo "DONE!!!"
             fi
             if test ! -z "$(docker images -q yi/tflow-build:0.7)"; then
                echo "Docker Image Already Exist!!!"
                  else
                pv -f /media/common/DOCKER_IMAGES/TFlow-Build/yi-tflow-build-ssh-0.7.tar | docker load
                docker tag cf5f331d8ca1 yi/tflow-build:0.7
                echo "DONE!!!"
             fi
                '''
            }
    }
	      stage('Test yi/tflow-build:0.X-python-v.3.6.3 Docker Image') { 
            steps {
                sh '''#!/bin/bash -xe
		              echo 'Hello, BUILD-DOCKER!!'
                    image_id="$(docker images -q yi/tflow-build:0.7-python-v.3.6.3)"
                      if [[ "$(docker images -q yi/tflow-build:0.7-python-v.3.6.3 2> /dev/null)" == "$image_id" ]]; then
                          docker inspect --format='{{range $p, $conf := .RootFS.Layers}} {{$p}} {{end}}' $image_id
                      else
                          echo "It appears that current docker image corrapted!!!"
                          exit 1
                      fi 
                   ''' 
            }
        }
		  stage('Test yi/tflow-build:0.X Docker Image') { 
            steps {
                sh '''#!/bin/bash -xe
		              echo 'Hello, BUILD-DOCKER!!'
                    image_id="$(docker images -q yi/tflow-build:0.7)"
                      if [[ "$(docker images -q yi/tflow-build:0.7 2> /dev/null)" == "$image_id" ]]; then
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
