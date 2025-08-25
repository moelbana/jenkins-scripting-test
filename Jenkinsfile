@Library('jenkins-shared-lib') _   // load your shared library

properties([
    parameters([
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build'),
        choice(name: 'BUILD_ENV', choices: ['dev', 'qa', 'prod'], description: 'Build environment'),
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run unit tests?')
    ])
])

node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build & Test in Parallel') {
        parallel(
            "Build": {
                buildUtils.build()
            },
            "Test": {
                if (params.RUN_TESTS) {
                    buildUtils.test()
                } else {
                    echo "Skipping tests as per parameter"
                }
            }
        )
    }

    stage('Post Build Info') {
        echo "Pipeline finished for branch: ${params.BRANCH_NAME} in ${params.BUILD_ENV} environment."
    }
}
