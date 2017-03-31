node {
    def dockerHome = tool name: "docker"
    env.PATH = "${dockerHome}/bin:${env.PATH}"

    stage("Checking out") {
        checkout scm
    }

    stage("Docker pull") {
        sh "docker pull thomasmunoz13/vup-testing"
    }

    stage("Build & Deploy") {
        withDockerContainer(image: "thomasmunoz13/vup-testing",
                toolName: "docker") {

            sh "mcs src/*.cs -pkg:wcf -out:server.exe"
        }
    }
    
    stage('deploy'){
        
        withDockerRegistry([credentialsId: "dockerhub-loick"]) {
            sh 'ls -la'
            sh "docker build -t loick111/tcf-dotnet:${BRANCH_NAME} ."
            sh "docker push loick111/tcf-dotnet:${BRANCH_NAME}"
        }
         
    }
}