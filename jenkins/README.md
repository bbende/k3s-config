# Jenkins

1) Followed two posts:
*  [How to Setup Jenkins on Kubernetes Cluster](https://devopscube.com/setup-jenkins-on-kubernetes-cluster)
* [Jenkins on Kubernetes: From Zero to Hero](https://medium.com/slalom-build/jenkins-on-kubernetes-4d8c3d9f2ece).

2) Modifications:
* Used a PVC from Longhorn so we don't need to deal with node affinity
* Used the `jenkins4eval/jenkins:latest` image for `arm64` support
* Added an ingress with the host `jenkins.private`
* Updated `/etc/hosts` mapping on laptop so `jenkins.private` can be accessed through control plane node.

3) Installed Kubernetes plugin in Jenkins. Required restarting Jenkins (https://jenkins.private/safeRestart).

4) In Jenkins Kubernetes Cloud Configuration:
* Set `Kubernetes Namespace` to `jenkins`
* Set `Jenkins URL` to `http://jenkins:8080`
* Set `Pod Label` to `jenkins = executor`
* Set `Defaults Provider Template Name` to `default-agent`
* Add `Pod Template` named `default-agent` in namespace `Jenkins`
* In `default-agent`, add a container template named `jnlp` with `Docker Image` of `pi4k8s/inbound-agent:4.3`, this overrides the default agent with an image that works on arm64

5) Create a Pipeline job with the following:
```
pipeline {
    agent {
        kubernetes {
            defaultContainer 'jnlp'
        }
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello everyone! This is now running on a Kubernetes executor!'
            }
        }
    }
}
```
