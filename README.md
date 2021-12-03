# Secure pipelines
The following procedures will define how to create a secure pipeline to build and deploy images.

## Prerequisites
Deploy Openshift Pipelines to the cluster.

## Chains
Once you have OpenShift pipelines deployed deploy Tekton Chains.

```
kubectl apply --filename https://storage.googleapis.com/tekton-releases/chains/latest/release.yaml
```

Using Cosign generate a secret.
```
cosign generate-key-pair k8s://tekton-chains/signing-secrets
```

## Pipelines
Create the tasks
```
kubectl create -f tasks
```
Create a secret for your registry. Note the registry must support image signing capabilities.

Create the pipeline
```
kubectl create -f pipeline
```

In the namespace in which the pipeline will run create the following secret based on a GitHub personal access token.
```
kubectl create secret generic image-updater-secret --from-literal=token=FASASDADSASD
```

## Base deployment
Create the base deployment. 

```
kubectl create -f kubernetes-objects