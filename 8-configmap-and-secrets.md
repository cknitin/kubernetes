# Kubernetes configmap and secrets

- While performing application deployments on K8s Cluster, sometimes we need to change the application Configuration file depending on environments like dev, QA, stage or Prod.

- Changing this application Configuration file means we need to change source Code, Commit the Change, Creating a new image and then go through the complete deployment process.
  
- Hence these configurations should be decoupled from image Content in order to keep Containerised application portable.

- This is where Kubernetes Configmap comes handy. It allows us to handle Configuration files much more efficiently.
