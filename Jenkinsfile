pipeline {
	agent any 
	stages {
		stage('git scm update') {
			steps {
				git url: 'https://github.com/dorong9/freestyle.git', branch: 'master'
			}
		}
		stage('docker build and push') {
			steps {
				sh '''
				docker build -t 211.183.3.100:5000/webtest:1.0 .
				docker push 211.183.3.100:5000/webtest:1.0
				'''
			}
		}
		stage('deployment and loadbalancer') {
			steps {
				sh '''
				kubectl create deploy pipeweb --image=211.183.3.100:5000/webtest:1.0
				kubectl expose deploy pipeweb --type=LoadBalancer --port=8080 \
				                              --target-port=80 --name=pipeweb-svc
				'''                              
			}
		}
	}
}
