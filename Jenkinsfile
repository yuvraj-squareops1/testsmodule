pipeline {
    agent any

    parameters {
        // Add the branch parameter to allow selecting the branch from the UI
        //string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'Enter the branch name to build')
        //extendedChoice(defaultValue: '', description: 'terraform module stacks', multiSelectDelimiter: ',', name: 'BRANCH_NAME', quoteValue: false, saveJSONParameterToFile: false, type: 'PT_SINGLE_SELECT', value: 'main,test,qa,dev', visibleItemCount: 4)
        choice(name: 'BRANCH_NAME',
               choices: ['main', 'test', 'qa', 'dev'],
               description: 'terraform module stacks',
               default: 'main')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the selected branch
                checkout([$class: 'GitSCM',
                          branches: [[name: "*/${params.BRANCH_NAME}"]],
                          userRemoteConfigs: [[url: 'https://github.com/yuvraj-squareops1/testsmodule.git']]])
            }
        }

        stage('Build') {
            steps {
                // Add the build steps here (e.g., compile, test, etc.)
                // sh 'mvn clean install'
                sh 'git branch -a'
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
