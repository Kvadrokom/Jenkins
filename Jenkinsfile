node {
    timestamps() {
        ansiColor('xtera') {
            try {
                properties([
                    parametries([
                        extendChoice(
                            defaultValue: 'chrome,firefox,sberbrowser',
                            multiSelectDelimiter: ',',
                            name: 'BROWSERS',
                            quoteValue: false
                            saveJSONParameterToFile: false,
                            type: 'PT_CHECKBOX',
                            value: 'chrome,firefox,edge,safari,sberbrowser'
                            visibleItemCount: 5])
                        )
                ])
                stage('Scripted parallel') {
                    Map<String, Closure> executers = [:]
//                     def browsers = ['chrome', 'safari', 'edge']
                    BROWSERS.tokenize(',').each { browser ->
                        executers[browser] = {
                            echo("Testing the ${browser} browser")
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
    }
}