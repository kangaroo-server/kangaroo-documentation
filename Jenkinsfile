/**
 * Jekyll check pipeline for our documentation site.
 */

pipeline {

    agent { label 'worker' }

    options {
        timeout(time: 30, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    stages {

        /**
         * Get environment statistics.
         */
        stage('Stat') {
            steps {
                script {
                    sh('''
                        env
                        /snap/bin/hugo version
                        pygmentize -V
                    ''')
                }
            }
        }

        /**
         * Test and build.
         */
        stage('Build') {
            steps {
                sh("/snap/bin/hugo")
            }
        }

        /**
         * Publish.
         */
        stage('Publish') {
            when {
                expression {
                    env.BRANCH_NAME == 'master'
                }
            }
            environment {
                GITHUB = credentials('krotscheck_jenkins_github_credentials')
            }
            steps {
                sh("""
                    # Go To Public folder
                    cd public

                    # Add changes to git.
                    git checkout master
                    git add .

                    # Commit changes.
                    git commit -m "Jenkins: Rebuilding site #${env.BUILD_NUMBER}"

                    # Push source and build repos.
                    git push https://${GITHUB_USR}:${GITHUB_PSW}@github.com/kangaroo-server/kangaroo-server.github.io.git master
                """)
            }
        }
    }

    post {

        /**
         * When the build status changed, send the result.
         */
        changed {
            script {
                def buildStatus = currentBuild.currentResult
                def url = env.BUILD_URL, message, color

                if (env.CHANGE_URL && buildStatus == 'SUCCESS') {
                    url = env.CHANGE_URL
                }

                switch (buildStatus) {
                    case 'FAILURE':
                        message = "Build <${url}|${env.JOB_NAME}> failed."
                        color = '#AA0000'
                        break
                    case 'SUCCESS':
                        message = "Build <${url}|${env.JOB_NAME}> passed."
                        color = '#00AA00'
                        break
                    case 'UNSTABLE':
                    default:
                        message = "Build <${url}|${env.JOB_NAME}> unstable."
                        color = '#FFAA00'
                }

                slackSend(
                        channel: '#build-notifications',
                        tokenCredentialId: 'kangaroo-server-slack-id',
                        teamDomain: 'kangaroo-server',
                        color: color,
                        message: message
                )
            }
        }

        /**
         * Actions always to run at the end of a pipeline.
         */
        always {
            /**
             * Delete everything, to keep track of disk size.
             */
            cleanWs(deleteDirs: true)
        }
    }
}
