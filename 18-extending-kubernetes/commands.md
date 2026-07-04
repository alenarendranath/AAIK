# Extending Kubernetes — kubectl Commands

## CRDs
```bash
kubectl get crds                                          # list all CRDs
kubectl get crd <name>
kubectl describe crd <name>
kubectl get crd <name> -o yaml                           # full spec with validation schema

# Once a CRD is installed, use it like any resource
kubectl get <plural-name>
kubectl get aaikdatabases
kubectl describe aaikdatabase <instance-name>
kubectl delete aaikdatabase <instance-name>

# Watch for CRD events
kubectl get events --field-selector involvedObject.kind=AaikDatabase
```

## Operators
```bash
# Install an operator (usually via OLM or Helm)
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml

# Check operator pods
kubectl get pods -n cert-manager

# Check operator CRDs it installed
kubectl get crds | grep cert-manager

# View operator logs
kubectl logs -n cert-manager deployment/cert-manager -f
```

## Admission Webhooks
```bash
kubectl get mutatingwebhookconfigurations
kubectl get validatingwebhookconfigurations
kubectl describe mutatingwebhookconfiguration <name>
kubectl describe validatingwebhookconfiguration <name>
```

## Metrics & Extensions
```bash
# Check API groups (find your custom API)
kubectl api-resources --verbs=list
kubectl api-resources | grep aaik

# Check API versions
kubectl api-versions | grep aaik.dev
```
