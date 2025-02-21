---
title: kubectl Utility
---

# kubectl

Interact with Rancher using kubectl.

### kubectl Utility

Install the `kubectl` utility. See [install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

Configure kubectl by visiting your cluster in the Rancher Web UI, clicking on `Kubeconfig`, copying contents, and putting them into your `~/.kube/config` file.

Run `kubectl cluster-info` or `kubectl get pods` successfully.

### Authentication with kubectl and kubeconfig Tokens with TTL

_Requirements_

If admins have [kubeconfig token generation turned off](../about-the-api/api-tokens.md#disable-tokens-in-generated-kubeconfigs), the kubeconfig file requires the [Rancher CLI](./rancher-cli.md) to be present in your PATH when you run `kubectl`. Otherwise, you’ll see an error like:
`Unable to connect to the server: getting credentials: exec: exec: "rancher": executable file not found in $PATH`.

This feature enables kubectl to authenticate with the Rancher server and get a new kubeconfig token when required. The following auth providers are currently supported:

1. Local
2. Active Directory (LDAP only)
3. FreeIPA
4. OpenLDAP
5. SAML providers: Ping, Okta, ADFS, Keycloak, Shibboleth

When you first run kubectl, for example, `kubectl get pods`, it will ask you to pick an auth provider and log in with the Rancher server. The kubeconfig token is cached in the path where you run kubectl under `./.cache/token`. This token is valid until [it expires](../about-the-api/api-tokens.md#disable-tokens-in-generated-kubeconfigs), or [gets deleted from the Rancher server](../about-the-api/api-tokens.md#deleting-tokens). Upon expiration, the next `kubectl get pods` will ask you to log in with the Rancher server again.