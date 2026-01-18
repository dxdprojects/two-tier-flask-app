@Library("Shared_Library") _
pipeline{
    agent { label "dev" };
    stages{
        stage("Code Clone"){
            steps{
                echo "Cloning code from github"
                script{
                    clone("https://github.com/dxdprojects/two-tier-flask-app.git", "master")
                }
            }
        }
        stage("Trivy Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t flask_app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("docker_credentials", "Two_Tier_Flask_App_Shared_Library")
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post {
        success{
            emailext(
                subject: "Build Success",
                body: "Your build was successful",
                to: "deeplearning740@gmail.com"
            )
        }
        failure{
             emailtext(
                subject: "Build Failed",
                body: "Your build has failed",
                to: "deeplearning740@gmail.com"
            )
        }
    }
}
