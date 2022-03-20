# TLS certificates for Traefik

Place the TLS certificate and key file (tls.crt and tls.key), and refer to those in ../configs/dynamic/config.yaml.

If using K8s, additionally uncomment the `traefik-certs` patch and secret generator in the kustomization.yaml.
