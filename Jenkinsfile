node{
    stage("SCM checkout"){
        git credentialsId: '5bbd6166-d9ae-4765-a20f-e6a97d94ba3d', url: 'https://github.com/yassinekoaubai/my-app.git'
    }
    
    stage("MVN package"){
        def mvnHome = tool name: 'maven-tool', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean package"
    }
    
    stage("Build Docker Image"){
        sh "docker build -t yassineka/my-app:1.0.0 ."
    }
    
    stage("Push Docker Image"){
        withCredentials([string(credentialsId: '27ea8483-6894-46fe-82e2-675ba6d60f45', variable: 'DockerHubPwd')]) {
            sh "docker login -u yassineka -p ${DockerHubPwd}"
        }
        sh "docker push yassineka/my-app:1.0.0"
    }
    
    stage("Run Container on Dev Server"){
        def dockerRun = "docker run -d -p 8080:8080 --name my-app yassineka/my-app:1.0.0"
        sshagent(['d55cdd8f-7beb-41e4-8fd4-c4d32c9b49e9']) {
            sh "ssh -o StrictHostKeyChecking=no retard@192.168.11.120 ${dockerRun}"
        }
    }
}