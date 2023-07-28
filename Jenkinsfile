// pipeline {
//     agent any

//     parameters {
//         // Add the branch parameter to allow selecting the branch from the UI
//         //string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'Enter the branch name to build')
//         extendedChoice(defaultValue: '', description: 'terraform module stacks', multiSelectDelimiter: ',', name: 'BRANCH_NAME', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_SINGLE_SELECT', value: 'main,test,qa,dev', visibleItemCount: 4)
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 // Checkout the selected branch
//                 checkout([$class: 'GitSCM',
//                           branches: [[name: "*/${params.BRANCH_NAME}"]],
//                           userRemoteConfigs: [[url: 'https://github.com/yuvraj-squareops1/testsmodule.git']]])
//             }
//         }

//         stage('Build') {
//             steps {
//                 // Add the build steps here (e.g., compile, test, etc.)
//                 // sh 'mvn clean install'
//                 sh 'git branch -a'
//             }
//         }
//     }

//     post {
//         success {
//             // Actions to be performed if the build is successful
//             echo 'Build successful!'
//         }
//         failure {
//             // Actions to be performed if the build fails
//             echo 'Build failed!'
//         }
//     }
// }



pipeline {
    parameters {
        extendedChoice(defaultValue: '', description: 'The Server Name', multiSelectDelimiter: ',', name: 'ServerName', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_CHECKBOX', value: 'lou-dev-notify-web2.hellospoke.com,lou-dev-notify-web1.hellospoke.com', visibleItemCount: 2)
    }
    agent any
    stages {
        stage('Fetch Branches') {
            steps {
                script {
                    // Get all branch names from the remote Git repository
                    def branchNames = sh(
                        script: 'git ls-remote --heads origin | cut -d / -f 3',
                        returnStdout: true
                    ).trim().split('\n')

                    // Format the branch names to be used in the choice parameter
                    def formattedBranches = branchNames.collect { branch ->
                        branch.trim()
                    }

                    // Add the choice parameter dynamically with the fetched branch names
                    properties([
                        parameters([
                            choice(name: 'BRANCH_NAME',
                                   choices: formattedBranches,
                                   description: 'Select the branch to build')
                        ])
                    ])
                }
            }
        }

        stage('Build') {
            steps {
                // Use the selected branch in the build steps
                checkout([$class: 'GitSCM',
                          branches: [[name: "*/${params.BRANCH_NAME}"]],
                          userRemoteConfigs: [[url: 'https://github.com/yuvraj-squareops1/testsmodule.git']]])

                // Add the build steps here (e.g., compile, test, etc.)
                // sh 'mvn clean install'
                sh 'git branch -a'
                sh 'ls -la'
            }
        }
        stage('Choose Fruit') {
            steps {
                script {
                    // Define the Extended Choice Parameter options
                    def ServerName = ["Apple", "Banana", "Orange", "Grapes", "Mango"]

                    // Use the input step to prompt the user for input
                    properties([
                        parameters: ([
                            extendedChoice(name: 'FRUIT_NAME',
                                description: 'Select your favorite fruit:',
                                multiSelectDelimiter: ',',
                                quoteValue: false,
                                type: 'PT_CHECKBOX',
                                value: ServerName,
                                visibleItemCount: 5)                                
                    // Use the selected fruit in the pipeline
                    echo "Selected Fruit: ${FRUIT_NAME}"
                ])
                            ])
                }
            }
        }
    }

    post {
        success {
            // Actions to be performed if the build is successful
            echo 'Build successful!'
        }
        failure {
            // Actions to be performed if the build fails
            echo 'Build failed!'
        }
    }
}
