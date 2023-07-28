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
        extendedChoice(
            name: 'FRUIT_NAME',
            description: 'Select your favorite fruit:',
            type: 'PT_SINGLE_SELECT',  // Use PT_SINGLE_SELECT for single-select
            value: 'Apple',            // Provide the selected value as a single string
            groovyScript: [
                sandbox: false,
                script: ['Apple', 'Banana', 'Orange', 'Grapes', 'Mango']
            ]
        )
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
                    // Access the selected fruit using ${params.FRUIT_NAME}
                    echo "Selected Fruit: ${params.FRUIT_NAME}"
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
