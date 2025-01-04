# Kubernetes ConfigMap Management - Command Reference Guide

### Project Content Table
- [Core ConfigMap Operations](#core-configmap-operations)
- [Advanced ConfigMap Management](#advanced-configmap-management)
- [Troubleshooting and Verification](#troubleshooting-and-verification)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/thetoriqul)  
> **Description**: Comprehensive guide for managing Kubernetes ConfigMaps  
> **Learning Focus**: Configuration management in Kubernetes environments  
> **Note**: Review and understand each command before execution in your environment.

## Core ConfigMap Operations

### Creating ConfigMaps

#### Imperative Approach
```bash
# Create ConfigMap with multiple key-value pairs
kubectl create configmap db-config \
    --from-literal=MYSQL_ROOT_PASSWORD=abc123 \
    --from-literal=MYSQL_USER=user1 \
    --from-literal=MYSQL_PASSWORD=user1@mydb

# Verify creation
kubectl get configmap db-config
```

#### Declarative Approach
```bash
# Apply ConfigMap from YAML file
kubectl apply -f config-map.yaml

# Verify creation
kubectl describe configmap db-config
```

### Viewing ConfigMaps

```bash
# List all ConfigMaps in current namespace
kubectl get configmap

# Get detailed information about specific ConfigMap
kubectl describe configmap db-config

# Export ConfigMap in YAML format
kubectl get configmap db-config -o yaml
```

## Advanced ConfigMap Management

### Updating ConfigMaps

```bash
# Edit ConfigMap directly
kubectl edit configmap db-config

# Patch specific values
kubectl patch configmap db-config \
    --patch '{"data":{"MYSQL_USER":"newuser"}}'

# Verify changes
kubectl get configmap db-config -o jsonpath='{.data}'
```

### Managing ConfigMap Volumes

```bash
# Create pod with ConfigMap as volume
kubectl apply -f pod-with-configmap-volume.yaml

# View mounted ConfigMap contents
kubectl exec [pod-name] -- ls /etc/config

# Check specific ConfigMap value in pod
kubectl exec [pod-name] -- cat /etc/config/MYSQL_USER
```

### Environment Variable Configuration

```bash
# Create pod with ConfigMap as env vars
kubectl apply -f pod-with-configmap-env.yaml

# Verify environment variables
kubectl exec [pod-name] -- env | grep MYSQL
```

## Troubleshooting and Verification

### Validation Commands

```bash
# Check ConfigMap syntax
kubectl apply -f config-map.yaml --dry-run=client

# Validate pod configuration
kubectl auth can-i create configmap

# View ConfigMap events
kubectl get events | grep configmap
```

### Cleanup Operations

```bash
# Delete specific ConfigMap
kubectl delete configmap db-config

# Remove all resources created by this project
kubectl delete -f .

# Verify cleanup
kubectl get configmap
```

## Learning Notes

1. ConfigMaps should be created before the pods that depend on them
2. Changes to ConfigMaps require pod recreation to take effect when used as environment variables
3. Volume-mounted ConfigMaps can be updated dynamically
4. Use namespaces to organize ConfigMaps in multi-environment setups
5. Always validate ConfigMap data before applying to production environments

---

> 💡 **Best Practice**: Store non-sensitive configuration in ConfigMaps and use Secrets for sensitive data

> ⚠️ **Warning**: ConfigMap data is stored as plain text. Never store sensitive information in ConfigMaps

> 📝 **Note**: Consider using helm or kustomize for managing ConfigMaps across environments