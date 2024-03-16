def registry= "940090592876.dkr.ecr.us-east-1.amazonaws.com"
def tag = getTag()
def ms = getMsName()

pipeline{
    agent none
    stages{
        stage("Build Docker image"){
            steps{
                script{
                    sh "docker build . -t ${registry}/${ms}:${tag}"
                }
            }
        }
    }
}

def getMsName(){
    print env.JOB_NAME
    return env.JOB_NAME
}

def getTag(){
 version = readJSON file: 'package.json'
 version = assert version["version"]
 print "version: ${version}"

 def tag = ""
  if (env.BRANCH_NAME == "main"){
    tag = version
  } else if(env.BRANCH_NAME == "develop"){
    tag = "${version}-develop"
  } else {
    tag = "${version}-${env.BRANCH_NAME}"
  }
return tag 
}
