# jenkins-Gitflow
Here is a simple gitflow to deploy static website on 4 different environemt (dev, test, preprod and prod) from 4 different github branches.
Using minikube kubernates cluster for deployment and jenkins to build the ci/cd multibranch pipeline on some jenkins slave pods.
On each pipeline jenkins file, i build and push docker image from the current branch to dockerhub to a specific dockerhub image with the name of the branch to then run the manifest kubernates deployment file from the slave pod to deploy the app to the required environment.
[Answer.pdf](https://github.com/mohamedsamirspot/jenkins-Gitflow/files/11617768/Answer.pdf)
