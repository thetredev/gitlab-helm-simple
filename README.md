# Simple GitLab Helm chart for self-hosted deployments
This Helm chart will deploy [GitLab using the Docker method](https://docs.gitlab.com/ee/install/docker.html) on Kubernetes. Hence it's simple, all-inclusive, easy to understand, and most importantly: NOT production ready!

## DISCLAIMER
This is NOT intended for production environments! Only use this if you want to quickly spin up GitLab in your Kubernetes cluster via Helm.

## This project will NOT provide
- scalability
- high availability
- good monitoring
- focus on security (mostly because of SSH, see below)

# Configuration
Have a look at the [values.yaml](deploy/values.yaml) file.

# SSH
I'm using the [Kubernetes NGINX Ingress Controller](https://github.com/kubernetes/ingress-nginx) on Kubernetes v1.25. I couldn't get [exposing the SSH TCP service](https://github.com/kubernetes/ingress-nginx/blob/main/docs/user-guide/exposing-tcp-udp-services.md) to work, so I'm relying on a `LoadBalancer` setup in [the service resource](deploy/templates/service.yaml). This will require some DNS/NAT magic on your end to make cloning and pusing repositories via SSH and URL rather than IP work as expected. I'm not a networking expert so I don't recommend that at all. It just works for my specific setup at the moment.

You might consider using the [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io) for exposing the SSH port. I'm not very familiar with that and I'm waiting for the API to graduate out of beta before using it.
