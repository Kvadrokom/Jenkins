node {
    timestamp()
    ansiColor('xtera')
    try {
        stage('Scripted parallel') {
            Map<String, Closure> executers = [:]
            def browsers = ['chrome', 'safari', 'edge']
            browsers.each { browsers ->
                executers[browsers] = {
                    echo("Testing the ${browsers} browser")
                }
            }
            parallel(executers)
        }
    }
    catch(Exception e) {
        echo ('I will allways say Hello only failure')
        echo(e.message.to_string())
    }
    finally {
        def currentResult = currentBuild.result ?: 'Success'
        switch (currentResult) {
            case 'UNSTABLE':
                echo('This wills run only if the run was marked unstable')
                break
            case 'Success':
                echo('I will allways say Hello only success')
                break
            default:
                echo("Current status ${currentResult}")
        }
        echo 'I will allways say Hello again'
        cleanWs()
    }
}