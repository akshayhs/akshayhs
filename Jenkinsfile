/* Variables to be used inside the pipeline */
def gitExecutable = sh(script: 'which git', returnStatus: true).trim()
def nodeExecutable = sh(script: 'which node', returnStdout: true).trim()
def npmExecutable = sh(script: 'which npm', returnStdout: true).trim()

pipeline {
	agent any

	stages {
		stage('Install git if it does not exist') {
			steps {
				script {
					/* Ensure git exists in the operating system. If it does not, install it. */
					if (gitExecutable != 0) {
						echo 'A git executable was not found on the server. Installing Git on the server.'
						sh 'sudo apt install -y git'
						echo "Git was successfully installed. The current git version is ${sh(script: 'git --version', returnStdout: true).trim()}"
					} else {
						echo 'A working version of Git is found on the server. Skipping the installation step.'
					}
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
					/* Ensure the node.js environment and npm is installed. Else, install it */
					if (nodeExecutable != 0 || npmExecutable != 0) {
						echo 'Node.js is not installed on the system.'
						echo 'Installing Node.js.'
						/* Download the setup file from the nodesource repo */
						sh 'curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -'
						/* Install Node.js */
						sh 'sudo apt install -y nodejs'
						echo 'Node.js has been installed.'
					}
					echo 'Node.js is already installed on the system. Skipping installation'
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
