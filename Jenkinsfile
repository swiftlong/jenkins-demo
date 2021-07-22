node(){
    stage('clone'){
        echo "1.Clone"
        git url: "https://github.com/swiftlong/jenkins-demo.git"
        script {
        build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
        }
    }
    stage('test'){
        echo "2.Code test"
        echo "test success!"
    }
    stage('Build'){
        echo "3.Build Docker image stage"
        sh "docker build -t swiftlong/jenkins-demo:${build_tag} ."
    }
    stage('Push'){
        echo "4.Push Docker Image Stage"
        withCredentials([usernamePassword(credentialsId: 'docker_hub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
            sh "docker push swiftlong/jenkins-demo:${build_tag}"
        }
    }
    stage('YAML'){
        echo "5. Change YAML File Stage"
        echo "${env.BRANCH_NAME}"
        if (env.BRANCH_NAME == 'master') {
            input "确认要部署线上环境吗？"
        }
        sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
        sh "sed -i 's/<BRANCH_NAME>/${env.BRANCH_NAME}/' k8s.yaml"
    }
}
