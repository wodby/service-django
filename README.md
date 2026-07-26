# Python (Django) service for Kubernetes on Wodby

Build and run Python (Django) applications on Kubernetes with Wodby.

This repository defines the Wodby service manifests and operational
configuration for Python (Django).

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Start with a boilerplate

Use one of the boilerplates exposed by this service to start with compatible
build configuration and Wodby CI:

- [Django boilerplate](https://github.com/wodby/django-boilerplate)

## Wodby stacks using this service

- [Django application stack](https://github.com/wodby/stack-django)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `django` |
| Type | Application service |
| Inherits from | [`python`](https://github.com/wodby/service-python) with version constraint `^1.0.0` |
| Application build | Git source connection enabled; Dockerfile: `Dockerfile`; boilerplates: Django boilerplate |
| Configuration | 1 generated or fixed tokens |
| Operations | 2 actions |

## Use this service

Use this service through [Django application stack](https://github.com/wodby/stack-django), or reference `django` from a
custom Wodby stack.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

The inherited database link keeps the component `DB_*` variables for
compatibility and also supplies a secret `DATABASE_URL`. Django boilerplates
use the URL as the authoritative deployed connection while retaining the
component variables as a fallback for older or custom applications.

## Background jobs

The `django-celery` derivative is selected by default. The standard Django
stack makes Valkey required and supplies its persistent connection through the
secret `CELERY_BROKER_URL` setting used by the boilerplate's Celery
configuration. The inherited `REDIS_HOST`, `REDIS_PORT`, and `REDIS_PASSWORD`
variables remain available for custom applications.

The derivative uses Celery's `CELERY_APP` CLI variable, which defaults to the
boilerplate's `myapp` package and can be overridden alongside `GUNICORN_APP`
for custom Django project layouts.

The broker is queue infrastructure rather than a disposable Django cache. The
standard stack therefore uses persistence and a `noeviction` policy, and the
boilerplate does not configure Django's cache to share the broker.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
