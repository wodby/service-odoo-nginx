# Nginx (Odoo) service for Kubernetes on Wodby

The public entry point of Odoo apps on Wodby: Nginx in front of Odoo, sending live
updates to Odoo's gevent port as Odoo's deployment guide describes.

This repository defines the Wodby service manifests and operational
configuration for Nginx (Odoo).

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [Odoo application stack](https://github.com/wodby/stack-odoo)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `odoo-nginx` |
| Type | Application service |
| Inherits from | [`nginx-proxy`](https://github.com/wodby/service-nginx-proxy) with version constraint `^1.0.0` |
| Service links | Odoo, required |
| Configs | `preset`: Odoo's proxy rules |
| Image | Stock Nginx image, no build |

## Use this service

Use this service through [Odoo application stack](https://github.com/wodby/stack-odoo), or reference `odoo-nginx`
from a custom Wodby stack with a `backend` link to an [Odoo service](https://github.com/wodby/service-odoo).

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
