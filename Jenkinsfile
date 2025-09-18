pipeline {
    agent any
    
    stages {
		stage('Email Sanity (emailext)') {
			steps {
				emailext(
					to: 's221245018@deakin.edu.au, harleyjack96@gmail.com',
      				from: 'harleyjack96@gmail.com',
      				subject: "emailext sanity @ ${env.JOB_NAME} #${env.BUILD_NUMBER}",
      				mimeType: 'text/html',
      				body: '<p>Hello from <b>emailext</b> (no attachments).</p>'
				)
			}
		}
		
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Hyper-fixation/8.2CDevSecOps.git'
            }
        }

		stage('Install Dependencies') {
		    steps {
			    bat 'npm install'
		    }
		}

		stage('Run Tests') {
		    steps {
			    bat 'npm test || exit /b 0'
			}
			post {
				always {
					emailext(
						to: 's221245018@deakin.edu.au, harleyjack96@gmail.com',
						subject: "Jenkins - Run Tests - ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
						from: 'harleyjack96@gmail.com',
						mimeType: 'text/html',
						body: """<p><b>Stage:</b> Run Tests</p>
	  							 <p><b>Status:</b> ${currentBuild.currentResult}</p>
		   						 <p><b>Job:</b> ${env.JOB_NAME} #${env.BUILD_NUMBER}</p>
								 <p><a href='${env.BUILD_URL}console'>View Console Log></a></p>""",
					)
				}
			}
		}

		stage('Generate Coverage Report') {
		    steps {
				bat 'npm run coverage || exit /b 0'
			}
		}

		stage('NPM Audit (Security Scan)') {
		    steps {
		        bat 'npm audit || exit /b 0'
		    }
			post {
				always {
					emailext(
						to: 's221245018@deakin.edu.au, harleyjack96@gmail.com',
						subject: "Jenkins - Security Scan - ${currentBuild.fullDisplayName} - ${currentBuild.currentResult}",
						from: 'harleyjack96@gmail.com',
						mimeType: 'text/html',
						body: """<p><b>Stage:</b> NPM Audit (Security Scan)</p>
	  							 <p><b>Status:</b> ${currentBuild.currentResult}</p>
		   						 <p><b>Job:</b> ${env.JOB_NAME} #${env.BUILD_NUMBER}</p>
								 <p><a href='${env.BUILD_URL}console'>View Console Log></a></p>""",
					)
				}
			}
	    }
    }
}
