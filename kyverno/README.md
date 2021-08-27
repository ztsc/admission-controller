# kyverno

Followed the guide to test out kyverno admission controller:

https://docs.google.com/document/d/1d2Qm47wjjoyGDT8v3_ijB1Q4mGYV5cncAQoQniiR414/edit#

Create registry credential secret
```
kubectl create secret docker-registry registry-credentials --docker-server=https://index.docker.io/v2/  --docker-username=gkovan --docker-email=gkovan@hotmail.com --docker-password=my-docker-pw -n kyverno
```

Followed the instructions in link above to configure the kyverno controller.

To see if the kyverno was able to register itself as a mutating and validating admission controller, run command:
```
oc get validatingwebhookconfigurations,mutatingwebhookconfigurations
```


## Status

The docker.io/gkovan/quarkus-hello-service image is signed.

The docker.io/gkovan/greeter image is not signed.

Both of these deployments worked (i.e. the pod was running on the cluster).

I expected the gkovan/greeter image deployment to fail since it is not signed.
