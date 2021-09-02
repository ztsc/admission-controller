# kyverno

Followed the guide to test out kyverno admission controller:

https://docs.google.com/document/d/1d2Qm47wjjoyGDT8v3_ijB1Q4mGYV5cncAQoQniiR414/edit#

Also see:  
* https://kyverno.io/docs/writing-policies/verify-images/  (experimental feature)
* https://kyverno.io/docs



Create registry credential secret
```
kubectl create secret docker-registry registry-credentials --docker-server=https://index.docker.io/v2/  --docker-username=gkovan --docker-email=gkovan@hotmail.com --docker-password=my-docker-pw -n kyverno
```

Followed the instructions in link above to configure the kyverno controller.

To see if the kyverno was able to register itself as a mutating and validating admission controller, run command:
```
oc get validatingwebhookconfigurations,mutatingwebhookconfigurations
```

After further testing, the `kyverno-service-account` does not need `priveleged` scc access.
/********  Ignore
To get kyverno working on OpenShift 4.7, I had to add the `kyverno-service-account` to the `priveleged` scc.
```
oc adm policy add-scc-to-user privileged -z kyverno-service-account
```

or add the following line to the `-users` array list for the `priveleged` scc:
```
 - 'system:serviceaccount:kyverno:kyverno-service-account'
```
*************/ 

Also, kyverno by default skips certain resources (namespaces) including the kyverno namespace. See: https://kyverno.io/docs/installation/#resource-filters
So if you add a policy (e.g. disallow-latest-tag) and then deploy the greeter-unsigned deployment in the `kyverno` namespace, then the deployment will work and the pod will start since the kyverno namespace is ignored.
A [BUG]was opened as this is a security vulnerability. See: https://github.com/kyverno/kyverno/issues/2356.


## Status

The docker.io/gkovan/quarkus-hello-service image is signed.

The docker.io/gkovan/greeter image is not signed.

Both of these deployments worked (i.e. the pod was running on the cluster).

I expected the gkovan/greeter image deployment to fail since it is not signed.

To apply these yamls use the command:
```
oc apply -f <file.yaml>
```

Alternatively, to create a pod that runs a docker image use command:
```
oc run <pod-name> --image=<image-url>
```

example:
```
oc run unsigned --image=docker.io/gkovan/greeter:latest
```
