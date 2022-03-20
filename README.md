# Portable OIDC Authentication stack

This repository provides a portable, pluggable and exportable OIDC stack to protect any application in both docker and Kubernetes flavours.

## Tech stack

1. Router: The opensource [Traefik](https://doc.traefik.io/traefik/) router is used to enforce authentication using its [`forwardAuth` middleware](https://doc.traefik.io/traefik/middlewares/http/forwardauth/).

2. OIDC Proxy: The [`traefik-forward-auth`](https://github.com/thomseddon/traefik-forward-auth) is used to connect to any OAuth 2.0/OIDC supported Identity Provider. Additionally, it supports authorization rules if needed.

3. `docker-compose`: A compose stack is provided that can be used as a base for integrating with other applications.

4. Kubernetes: A kustomization manifest is provided that uses the configs and static manifests to deploy the same stack in Kubernetes with either a NodePort or integrating to an existing Ingress controller.

## Usage

This repository deploys a sample app using [`containous/whoami`](https://github.com/traefik/whoami) which also helps in visalization of the headers that will be available after login.

1. Modify the [`configs/oidc.env`](configs/oidc.env) and [`configs/oidc.secret.env`](configs/oidc.secret.env) to set the Identity Provider information. More details are available in [`traefik-forward-auth`'s configuration guide](https://github.com/thomseddon/traefik-forward-auth#configuration).

2. If you need HTTPS with a valid SSL certificate in case of docker, or using NodePort in Kubernetes (not needed when using Ingress), follow the steps below.
    * Add `tls.crt` and `tls.key` files to the [`certs/`](certs/) folder.
    * Modify the [`configs/dynamic/config.yaml`](configs/dynamic/config.yaml) to uncomment the TLS section on top.
    * In case of Kubernetes, uncomment the `traefik-certs` in the `patchesStrategicMerge` and `secretGenerator` section of [`kustomization.yaml`](kustomization.yaml).

3. Deploy the stack. For Kubernetes, this deploys to the default namespace. You can change this in the [`kustomization.yaml`](kustomization.yaml).
    * Docker: `docker-compose up -d`
    * Kubernetes: `kubectl apply -k .`
