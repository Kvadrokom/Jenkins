node {
    gitUrl = "https://github.com/Kvadrokom/Jenkins.git"
    gitBranches = "git ls-remote --heads ${gitUrl}".execute().text.readLines().collect { it.split()[1].replaceAll("refs/heads/", "") }.sort().reverse()
    timestamps() {
        ansiColor('xtera') {
            try {
                properties([
                    parameters([
                        extendedChoice(
                            defaultValue: 'chrome,firefox,sberbrowser,edge,safari',
                            multiSelectDelimiter: ',',
                            name: 'BROWSERS',
                            quoteValue: false,
                            saveJSONParameterToFile: false,
                            type: 'PT_CHECKBOX',
                            value: 'chrome,firefox,edge,safari,sberbrowser',
                            visibleItemCount: 5
                        )
                    ])
                    parameters([
                        extendedChoice(
                            defaultValue: 'master',
                            multiSelectDelimiter: ',',
                            name: 'branch',
                            quoteValue: false,
                            saveJSONParameterToFile: false,
                            type: 'PT_SINGLESELECT',
                            value: ${gitBrances},
                            visibleItemCount: ${gitBranches}.size()
                        )
                    ])
                ])
                stage('Scripted parallel') {
                    Map<String, Closure> executers = [:]
//                      def browsers = ['chrome', 'safari', 'edge']
                        params.BROWSERS.tokenize(',').each { browser ->
                        executers[browser] = {
                            echo("Testing the ${browser} browser")
                        }
                    }
                    parallel(executers)
                }
            }
            catch(Exception e) {
                echo ('I will allways say Hello only failure')
                echo(e.message.toString())
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