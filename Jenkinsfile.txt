pipeline {
    agent any
        stages {

            stage('One') {
                steps {
                    echo 'Hi from Start my pipeline'
                }
            }

            stage('Two') {
                steps {
                    input('Do you wanna proceed?')
                }
            }

            stage('Three') {
                when {
                    not {
                        branch 'master'
                    }
                }
                steps {
                    echo 'Hello'
                }
            }

            stage('Four') {
                parallel {
                    stage('Unit test') {
                        steps {
                            echo 'Running the Unit test ...'
                        }
                    }   
                    stage('Integration Test') {
                        agent {
                            docker {
                                reuseNode false
                                image 'Busybox'
                            }
                        }
                        steps {
                            echo 'Running the Integration test ...'
                        }
                    }     
            }
        }
}
