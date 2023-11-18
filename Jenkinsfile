int len(lst){
    int sz = 0
    for (line in lst) {
        sz++
    }
    return sz
}

String convertYamlToString(file) {
    String fileContents = new File(file).getText('UTF-8')
    lines = fileContents.readLines()
    ln = ''
    for (int i = 1; i < len(lines); ++i)
        ln = ln + lines[i].replaceAll("  - ", ",")
    return ln.substring(1, len(ln))
}

node {
    timestamps() {
        ansiColor('xtera') {
            try {
                checkout scm
                sh("ls -lha ${env.WORKSPACE}")
                lst = ['linux', 'macos', 'windows']
                print('Hi from try')
                String fileContents = new File("${env.WORKSPACE}/browsers.yml").getText('UTF-8')
                lines = fileContents.readLines()
                def numberValues = len(lines) - 1
                def listBrowsers = convertYamlToString("${env.WORKSPACE}/browsers.yml")
                sh("cat ${env.WORKSPACE}/browsers.yml")
                print('Hi after read file')
                print(listBrowsers)
                properties([
                    parameters([
                        extendedChoice(
                            defaultValue: listBrowsers,
                            multiSelectDelimiter: ',',
                            name: 'BROWSERS',
                            quoteValue: false,
                            saveJSONParameterToFile: false,
                            type: 'PT_CHECKBOX',
                            value: listBrowsers,
                            visibleItemCount: numberValues
                        )
                    ])
                ])
                stage('matrix') {
                    print('Hi from stage matrix')
                    str = ''
                    Map<String, Closure> executers = [:]
                    for(str in lst) {
                        params.BROWSERS.tokenize(',').each { browser ->
                        executers[browser] = {
                            echo("Testing ${str} the ${browser} browser")
                        }
                    }
                }
//                 parallel(executers)
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