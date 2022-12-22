# k8s-oauth2-proxy-example

This repo illustrates how to setup shared Google Authentication service for multiple
deployments on Kubernetes platform

## Requirements

- Kubectl installed
- Working K8s cluster
- Nginx-ingress controller
- Cert-Manager controller
- External-DNS controller

## Usage

First, make sure your current `kubectl` namespace is setup and working correctly!

Create new namespaces:

```bash
kubectl create namespace oauth2-proxy
kubectl create namespace app1
kubectl create namespace app2
```

Configure your primary domain for experuments. We have `supercorp.com` in all k8s manifests,
so make sure to replace the value everywhere.

Configure oauth2-proxy env var secrets, `OAUTH2_PROXY_CLIENT_ID`, `OAUTH2_PROXY_CLIENT_SECRET`
and `OAUTH2_PROXY_COOKIE_SECRET` values in `oauth2-proxy/secret.yml` file.

Create oauth-proxy:

```bash
kubectl -n oauth2-proxy apply -f ./oauth2-proxy
```

Verify your `auth.supercorp.com` endpoint is working and redirects to Google login screen.

Next, create app1/app2 resources:

```bash
kubectl -n app1 apply -f ./app1
kubectl -n app1 apply -f ./app2
```

These should be working as `app1.supercorp.com` and `app2.supercorp.com` and redirect
to `auth.supercorp.com` upon first load. Auth expires in 4 hours.

That's it.
