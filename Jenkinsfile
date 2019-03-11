node {
        // Clean workspace before doing anything
    deleteDir()

    try {
        stage (Grab console) {
        def jenkinsBase = 'http://localhost:8080' // Set to Jenkins base URL
        def jenkinsJob =  'test-job' // Set to Jenkins job name
        def address = null
        def response = null
        def start = 0 // Start at offset 0
        def cont = true // This semaphore holds the value of X-More-Data header value
        try {
          while (cont == true) { // Loop while X-More-Data value is equal to true
            address = "${jenkinsBase}/job/${jenkinsJob}/lastBuild/logText/progressiveText?start=${start}"
            println("${address}")
            def urlInfo = address.toURL()
            response = urlInfo.openConnection()
            if (response.getResponseCode() != 200) {
                throw new Exception("Unable to connect to " + address) // Throw an exception to get out of loop if response is anything but 200
            }
            if (start != response.getHeaderField('X-Text-Size')) { // Print content if the starting offset is not equal the value of X-Text-Size header
              response.getInputStream().getText().eachLine { line ->
                println(line)
              }  
            }
            start = response.getHeaderField('X-Text-Size') // Set new start offset to next byte
            cont = response.getHeaderField('X-More-Data') // Set semaphore to value of X-More-Data field. If this is anything but true, we will fall out of while loop
            sleep(3000) // wait for 3 seconds 
          }
        }
        catch (Exception ex) {
          println (ex.getMessage()) 
        }
        }
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
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}
