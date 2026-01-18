pipeline{
    agent { label "dev" };
    stages{
        stage("Code Clone"){
            steps{
                echo "Cloning code from github"
                git url: "https://github.com/dxdprojects/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t flask_app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId: "docker_credentials", passwordVariable: "dockerPass", usernameVariable: "dockerUser")]){
                    sh "docker login -u ${env.dockerUser} -p ${env.dockerPass} "
                    sh "docker image tag flask_app ${env.dockerUser}/flask_app"
                    sh "docker push ${env.dockerUser}/flask_app"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
