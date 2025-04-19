# Install Azure CLI Extensions

Get a list of installed Azure CLI extensions:

```powershell
az extension list -o table
```

Get a list of available Azure CLI extensions:

```powershell
az extension list-available -o table
```

## ContainerApp Extension for Azure CLI

- **Purpose**: Enables management of Azure Container Apps, a serverless container platform for building and deploying modern apps with features like autoscaling, Dapr integration, and KEDA triggers.
- **Use Case**: Useful for deploying microservices or event-driven apps without managing Kubernetes, aligning with your interest in streamlined Azure infrastructure setups.
- **Install Command**:
    ```powershell
    az extension add --name containerapp --upgrade
    ```

