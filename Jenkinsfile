pipeline {
	agent any

	stages {
		stage('Install git if it does not exist') {
			steps {
				script {
					/* Ensure git exists in the operating system. If it does not, install it. */
					echo 'Installing Git on the server.'
					sh 'sudo apt install -y git'
					echo "Git was successfully installed. The current git version is ${sh(script: 'git --version', returnStdout: true).trim()}"
				}
			}
		}
		stage('Clone the repository from remote') {
			steps {
				script {
					/* Print a message about initiating the cloning procedure */
					echo 'Cloning the repository to system storage.'
					sh 'git clone https://github.com/akshayhs/akshayhs.git'
					/* Print a message successfully indicating the repository to be cloned to the system */
					echo 'Successfully cloned the repository to system storage.'
				}
			}
		}
		stage('Install Node.js for installing the linter package') {
			steps {
				script {
					echo 'Node.js is not installed on the system.'
					echo 'Installing Node.js.'
					/* Download the setup file from the nodesource repo */
					sh 'curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -'
					/* Install Node.js */
					sh 'sudo apt install -y nodejs'
					echo 'Node.js has been installed.'					
					echo "Currently available version for Node is ${sh(script: 'node -v', returnStdout: true).trim()}"
					echo "Currently available version for Node Package Manager(NPM) is ${sh(script: 'npm -v', returnStdout: true).trim()}"
				}
			}
		}
		stage('Install markdownlint from npm') {
			steps {
				sh 'npm install markdownlint'
			}
		}
		stage('Lint the markdown file') {
			steps {
				sh 'markdownlint ./README.md'
			}
		}
	}
}
