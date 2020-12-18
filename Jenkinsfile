def chartPipelineTasks = [:]
{

    node(label) {


        // array of changed charts (folder), empty by default
        def changedCharts = null

        if (env.BRANCH_NAME == 'master') {
            stage('Test') {
                withCredentials([usernamePassword(credentialsId: 'tcgibennett', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    container('git') {
                                echo "Search for changes in "

                                changed_directories = sh(
                                        script: """
                            git remote add github https://${env.GIT_USERNAME}:${env.GIT_PASSWORD}@github.com/tcgibennett/gateway-test.git;
                            git fetch github master;
                            CHANGED_FOLDERS=`git diff --find-renames --name-only \$(${getChangedFolderGitCommand(gitBranch)}) ./ | awk -F/ '{print \$1"/"\$2}' | uniq`;
                            if [ -z \"\$CHANGED_FOLDERS\" ]; then
                            printf \"No changes to charts found\";
                            exit 0;
                            fi;
                            for directory in \${CHANGED_FOLDERS}; do
                            if [ -d \${directory} ]; then
                                echo \${directory};
                            fi
                            done;
                            """,
                                        returnStdout: true
                                )
                    }
                }
            }
        } 
    }
}

try {
    parallel chartPipelineTasks
} catch(err) {
    echo "Error handled: ${err}"
}