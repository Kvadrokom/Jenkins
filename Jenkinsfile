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
                print('Hi from try')
                def listBrowsers = convertYamlToString("${env.WORKSPACE}/browsers.yml")
                sh("cat ${env.WORKSPACE}/browsers.yml")
                print('Hi after read file')
                properties([
                    parameters([
                        extendedChoice(
                            defaultValue: listBrowsers.browsers.join(','),
                            multiSelectDelimiter: ',',
                            name: 'BROWSERS',
                            quoteValue: false,
                            saveJSONParameterToFile: false,
                            type: 'PT_CHECKBOX',
                            value: listBrowsers.browsers.join(','),
                            visibleItemCount: 5
                        )
                    ])
                ])
//                 properties([
//                     parameters([
//                         extendedChoice(
//                             defaultValue: 'master',
//                             multiSelectDelimiter: ',',
//                             name: 'branch',
//                             quoteValue: false,
//                             saveJSONParameterToFile: false,
//                             type: 'PT_SINGLESELECT',
//                             value: ${branches},
//                             visibleItemCount: ${sz}
//                         )
//                     ])
//                 ])
                stage('Scripted parallel') {
                    print('Hi from stage parallel')
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