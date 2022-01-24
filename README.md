# SoapBox Labs - Technical Test

This is an example fastapi helm chart for basic deployment on a minikube cluster.

## Prerequisites
- Minikube / minikube cli.  It can be downloaded from [this link.](https://minikube.sigs.k8s.io/docs/start/)
- Helm cli tool.

## Note
We will be demonstrating traffic routing with a NodePort service.  To access this on Mac/ Darwin OS, \
we need a specific `minikube` command and a terminal needs to be open.

### Deployment steps.

1. Start the cluster `minikube start`.  This takes a few minutes.
1. Enable the metrics-server because we have a horisontal pod autoscaler. `minikube addons enable metrics-server`
1. Install the Helm chart by running this command from the root of the project `helm install soap-test ../fastapi`.
1. Get the URL from minikube `minikube service soap-test-fastapi --url`.
1. Copy the output (something like http://127.0.0.1:51003) and paste it into the browser.
1. Default response from fastapi server should be seen.
1. Check that env vars have been set properly:\
`kubectl exec -it $(kubectl get pod -l app.kubernetes.io/name=fastapi -o jsonpath="{.items[0].metadata.name}") -- env | grep -E '(ENV|LOCA|USER|PASS)'`
1. Check that HPA is configured.  Scaling can be viewed by putting some load on the server.\
`while true; do curl <output from minikube service soap-test-fastapi --url>; done`\
Having 3 running simultaneously (either in background or separate terminal windows) should be enough to trigger the HPA.
 
