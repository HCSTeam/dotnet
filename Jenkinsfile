node {
    def dockerHome = tool name: "docker"
    env.PATH = "${dockerHome}/bin:${env.PATH}"

    stage("Checking out") {
        checkout scm
    }

    stage("Docker pull") {
        sh "docker pull mono:3.10"
    }

    stage("Compile") {
        withDockerContainer(image: "mono:3.10",
                toolName: "docker") {

            sh "mcs src/*.cs -pkg:wcf -out:server.exe"
        }
    }

    stage("Build & Deploy") {
        withDockerRegistry([credentialsId: "dockerhub-loick"]) {
            sh "docker build -t loick111/tcf-dotnet:${BRANCH_NAME} dotNet/."
            sh "docker push loick111/tcf-dotnet:${BRANCH_NAME}"
        }
    }
}
