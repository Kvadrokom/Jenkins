node('none') {
    try {
        stage('Example') {
            echo 'Hello world!'
            def browsers = ['chrome', 'safari', 'edge']
            for (int i = 0; i < browsers.size(); i++)
                echo 'Testing ${browsers[i]} browser'
        }
    }
    catch(Exception e) {
        echo ('I will allways say Hello only failure')
        echo(e.message.to_string())
    }
    finally {
        def currentResult = currentBuild.result
        switch (currentResult) {
            case 'UNSTABLE':
                echo('This wills run only if the run was marked unstable')
                break
            case 'Success':
                echo('I will allways say Hello only success')
                break
            default:
                echo('Current status ${currentResult}')
        }
        echo 'I will allways say Hello again'
        cleanWs()
    }
}