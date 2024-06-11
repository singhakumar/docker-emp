pipeline {
    agent { label 'jen-agent-00' } // none or any 

    options {
        skipStagesAfterUnstable()
    }
    
    tools {
        maven 'm-version-3.9.7' 
    }
    
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who  I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    stages {
        stage('Non-Parallel Stage') {
            options {
                timeout(time:1,unit: 'HOURS')   // Set a timeout period for this stage, after which Jenkins should abort the stage.
                retry(5)
            }
            
            input {
                message "Should we continue ?"
                ok "Yes, we should."
                submitter "alice,bob"
                parameters {
                    string(name: 'YES_NO', defaultValue: 'NO', description: 'Who should I say hello to?')
                    choice(name: 'sex', choices: ['Male', 'Female', 'Transh'], description: 'Pick something')
                }
            }
            
            steps {                
                echo "Hello ${params.PERSON}"     
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"
            }
        }

// sequential
        stage('Sequential') {
            agent {
                label 'for-sequential'
            }
            environment {
                FOR_SEQUENTIAL = "some-value"
            }
            stages {
                stage('In Sequential 1') {
                    steps {
                        echo "In Sequential 1 ${params.YES_NO}"
                    }
                }
                stage('In Sequential 2') {
                    steps {
                        echo "In Sequential 2 branch name ${BRANCH_NAME}"
                    }
                }
                stage('Parallel In Sequential') {
                    parallel {
                        stage('In Parallel 1') {
                            steps {
                                echo "In Parallel 1"
                            }
                        }
                        stage('In Parallel 2') {
                            steps {
                                echo "In Parallel 2"
                            }
                        }
                    }
                }
            }
        } 

        stage('Parallel Stage') {
    /*        when {
                    branch 'master'         // Not working as master and main
                } 
            failFast true                   // parallel stages to all be aborted when any one of them fails, by adding failFast true to the stage containing the parallel
            
            or
            options {
            parallelsAlwaysFailFast()
            } */

            parallel {                      // Stages in Declarative Pipeline may have a parallel section containing a list of nested stages to be run in parallel.
                stage('Branch A') {
                    agent {
                       label 'jen-agent-02'
                    }
                    steps {
                        echo "On Branch A"
                    }
                }
                stage('Branch B') {
                    agent {
                        label 'jen-agent-02'
                    }
                    steps {
                        echo "On Branch B"
                    }
                }
                stage('Branch C') {
                    agent {
                        label 'jen-agent-02'
                    }
                    stages {
                        stage('Nested 1') {
                            steps {
                                echo "In stage Nested 1 within Branch C"
                            }
                        }
                        stage('Nested 2') {
                            steps {
                                    sh 'mvn --version'
                                    sh 'ip a'
                            }
                        }
                    }
                }

            }
        }
    }
    post {
        always {
            echo "I will always run"
        }
    }
}