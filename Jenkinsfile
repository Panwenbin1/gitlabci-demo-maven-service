#!groovy

@Library('jenkinslib')
def tools = new org.devops.tools()

String buildType = "${env.buildType}"
String buildShell = "${env.buildShell}"
String srcUrl = "${env.srcUrl}"

pipeline {
  agent { node { label "master" } }
  stages {
    stage("CheckOut"){
        steps {
            script {
                if("${runOpts}"== "GitlabPush"){
                    branchName = branch - "refs/heads/"
                }else {
                    String branchName = "#{env.branchName}"
                }
                println(branchName)
                
                tools.PrintMes('分支切换','red')
                checkout([$class: 'GitSCM', branches: [[name: "${branchName}"]], extensions: [], userRemoteConfigs: [[credentialsId: 'ef20a297-5755-4381-9e1e-c6f527864a78', url: '${srcUrl}']]])
            }
        }
    }
      
    stage("Build"){
        steps{
            script {
                mvnHome = tool "M2"
                sh "${mvnHome}/bin/mvn -v"
            }
        }
    }
  }
  
  post {
      success {
          script{
            tools.PrintMes('发布完成','green') 

          }
      }
      
  }
}
