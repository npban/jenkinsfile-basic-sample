node {
        // Clean workspace before doing anything
    deleteDir()

    try {
        stage ('Clone') {
                checkout scm
        }
        stage ('Build') {
                sh "echo 'shell scripts to build project...'"
        }
        stage ('Tests') {
                parallel 'static': {
                    sh "echo 'shell scripts to run static tests...'"
                },
                'unit': {
                    sh "echo 'shell scripts to run unit tests...'"
                },
                'integration': {
                    sh "echo 'shell scripts to run integration tests...'"
                }
        }
        stage ('Deploy') {
            sh "echo 'shell scripts to deploy to server...'"
        }
        stage('save log build') {
    		def logContent = Jenkins.getInstance()
    						.getItemByFullName(env.JOB_NAME)
    						.getBuildByNumber(Integer.parseInt(env.BUILD_NUMBER))
    						.logFile.text
                // copy the log in the job's own workspace
                writeFile file: "job_"+env.JOB_NAME+"_build_"+env.BUILD_NUMBER+"_log.txt", text: logContent
        }
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}
