// CODE_CHANGES = getGitChanges() You can specify functions as such. And then can be used on stages.
pipeline {
    agent { docker { image 'python:3.5.1' } } //docker plugin must be installed in order to use it as an agent
    environment { //env variables can store values in order to use them through out stages
        CUSTOM_ENV_VARIABLE = 'custom value'
        SERVER_CREDENTIALS = credentials('server-credentials')
    }
    tools { //provide tool names in quotes in order to use the tool through out different stages
        maven 'Maven'
        gradle 'Gradle'
        jdk 'Jdk'
        git 'Git'
        ant 'Ant'
        docker 'Docker'
    }
    parameters { //A set of parameter examples. You can either use string or choice. Not both at the same time.
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description:'')
        booleanParam(name:'executeTest', defaultValue: true, description:'')
    }
    stages {
        stage('init') {
            steps {
                groovy = load 'script.groovy' //Before using external groovy file it needs to be assigned to a variable.
            }
        }
        stage('build') {
            steps {
                script { //groovy scripts can only run under the script field.
                    groovy.buildApp()
                }
                when {
                    BRANCH_NAME == 'main' && CODE_CHANGES == true
                }
                sh 'python3 --version'
                echo 'build stage...'
                echo "this is the custom variable: ${CUSTOM_ENV_VARIABLE}"
            }
        }
        stage('test') {
            when {
                BRANCH_NAME == 'main' || BRANCH_NAME == 'master'
            }
            steps {
                script {
                    groovy.testApp()
                }
                echo 'test stage...'
            }
        }
        stage('deploy') {
            when {
                expression {
                    params.executeTest
                }
            }
            steps {
                script {
                    groovy.deployApp()
                }
                echo 'deploy stage...'
                // "server-credentials" is an "id" defined from ui in manage credentials section of manage jenkins
                withCredentials([
                    usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
                ]) {
                    sh "some script ${USER} ${PWD}"
                }
                echo "deploying version ${params.VERSION}"
            }
        }
    }
    post {
        always {
            echo 'always executed after the execution of the stages regardless the success of the pipeline'
            echo 'jenkins environment variables can be found here: http://localhost:8080/env-vars.html/ '
        }
        success {
            echo 'actions here are executed if the pipeline has successfully executed'
        }
        failure {
            echo 'actions here are executed if the pipeline has failed'
        }
    }
}
