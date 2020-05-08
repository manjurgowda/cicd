pipeline {
   agent any

   stages {
      stage('checkout_Code_integration') {
         steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/manjurgowda/cicd.git']]])
         }
      }
      stage('Unit_Testing') {
         steps {
                        sh "/usr/local/bin/pip install -r requirements.txt"
            sh "/usr/bin/python -m pytest -v tests/test_generator.py"
         }
      }
      stage('Docker_image_Build') {
         steps {
            sh "/usr/bin/docker build -t manjurgowda/cicd:latest ."
         }
      }
      stage('publish') {
         steps {
			withDockerRegistry(credentialsId: 'e26d476e-7df0-4862-9575-ab9002b6c3f8', url: 'https://index.docker.io/v1/') {
                     sh "/usr/bin/docker push manjurgowda/cicd:latest"
         }
         }
      }
      stage('Running_image_from_DockerHub') {
         steps {
           sh "nohup /usr/bin/docker run -p 5000:5000 --rm manjurgowda/cicd:latest &"
         }
         }
      }
   }

