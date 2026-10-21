# platform — Kubernetes (advanced)

A platform with a stateful database, cache, autoscaling API, and network isolation.

## Structure
```
platform/
├── kustomization.yaml     # applies everything, in order
├── namespace.yaml
├── rbac/                  # serviceaccount + role + rolebinding
├── config/               # configmap, secret
├── postgres/             # headless service + statefulset (per-pod disk)
├── redis/                # deployment + service
├── api/                  # deployment (SA + init container), service, hpa, pdb
├── web/                  # configmap + deployment + service (nginx proxy)
├── network/              # ingress (path routing) + networkpolicy
└── jobs/                 # one-off migration Job
```

## Deploy
```bash
kubectl apply -k .                 # applies the whole project via kustomize
kubectl get all -n platform
```
Open it: `curl http://platform.local/` and `curl http://platform.local/api/`.

## Tear down
```bash
kubectl delete -k .
# or: kubectl delete namespace platform
```

Covers: RBAC, StatefulSet + headless Service + volumeClaimTemplates, HorizontalPodAutoscaler, PodDisruptionBudget, NetworkPolicy, init container, Job, ConfigMap/Secret, Ingress path routing, probes, resources.
