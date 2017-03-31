node {

    stage("Checking out") {
        checkout scm
    }

    stage("Docker pull") {
        sh "docker pull mono:3.10"
    }

    stage("Compile") {
        withDockerContainer(image: "mono:3.10",
                toolName: "docker") {

            sh "mcs dotNet/src/*.cs -pkg:wcf -out:dotNet/server.exe"
        }
    }

    stage("Build & Deploy") {
        withDockerRegistry([credentialsId: "dockerhub-loick"]) {
            sh "docker build -t loick111/tcf-dotnet:${BRANCH_NAME} dotNet/."
            sh "docker push loick111/tcf-dotnet:${BRANCH_NAME}"
        }
    }
}
