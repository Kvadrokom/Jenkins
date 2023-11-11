node {
    def gitUrl = "https://github.com/Kvadrokom/Jenkins.git"
    def gitBranches = "git ls-remote --heads ${gitUrl}".execute().text.readLines().collect { it.split()[1].replaceAll("refs/heads/", "") }.sort().reverse()
//     def sz = ${gitBranches}.size()
//     def branches = ${gitBranches}.ids.join(" ")
    timestamps() {
        ansiColor('xtera') {
            try {

                listBrowsers = readFile("browsers.yml")

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
                    print(BROWSERS)
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