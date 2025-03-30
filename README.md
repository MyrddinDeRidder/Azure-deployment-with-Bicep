# Azure Container Deployment with Bicep

This repository contains Bicep templates for deploying containers to Azure Container Instances (ACI). The template provides a streamlined way to deploy containerized applications with customizable configurations.

## 🚀 Features

- Single container deployment to Azure Container Instances
- Support for both public and private container registries
- Configurable CPU and memory resources
- Public IP address exposure
- Custom port mapping
- Container registry authentication

## 📋 Prerequisites

- Azure CLI installed
- Azure subscription
- Resource group created in Azure
- (Optional) Access to Azure Container Registry (ACR) or another container registry

## 🛠️ Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `location` | Azure region for deployment | Resource group location |
| `containerGroupName` | Name of the container group | 'my-container-group' |
| `containerName` | Name of the container | 'crud' |
| `acrName` | Name of Azure Container Registry | 'yourAcrName' |
| `acrLoginServer` | Login server for ACR | '${acrName}.azurecr.io' |
| `imageName` | Full path to container image | '${acrLoginServer}/your-image:tag' |
| `acrUsername` | Username for container registry | '' |
| `acrPassword` | Password for container registry | '' |

## 📥 Installation

1. Clone this repository:
```bash
git clone <repository-url>
```

2. Navigate to the project directory:
```bash
cd <project-directory>
```

## 🚀 Deployment

### Using Public Image

```bash
az deployment group create \
  --resource-group <your-resource-group> \
  --template-file container.bicep \
  --parameters imageName='mcr.microsoft.com/azuredocs/aci-helloworld:latest'
```

### Using Private Azure Container Registry

```bash
az deployment group create \
  --resource-group <your-resource-group> \
  --template-file container.bicep \
  --parameters \
    acrName='yourAcrName' \
    imageName='yourAcrName.azurecr.io/your-image:tag' \
    acrUsername='<acr-username>' \
    acrPassword='<acr-password>'
```

## 🔑 Getting ACR Credentials

If you're using Azure Container Registry, you can get the credentials using these commands:

```bash
# Get ACR login server
az acr show --name <acr-name> --query loginServer

# Get ACR credentials
az acr credential show --name <acr-name>
```

## 📝 Container Image Format

Use the correct format for your container image based on the registry:

- Docker Hub: `docker.io/username/image:tag`
- Azure Container Registry: `yourregistry.azurecr.io/image:tag`
- Microsoft Container Registry: `mcr.microsoft.com/image:tag`

## 🏗️ Resource Configuration

The deployment creates:
- A container group in the specified resource group
- A single container with:
  - 1 CPU core
  - 1 GB memory
  - Public IP address
  - Port 80 exposed
  - Linux OS
  - "Always" restart policy

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## ⚠️ Important Notes

- Ensure your container image exists in the registry before deployment
- Keep your registry credentials secure
- Review and adjust resource requests (CPU/memory) based on your application needs
- Consider using Azure Key Vault for storing registry credentials in production

## 🆘 Troubleshooting

Common issues:
1. **Authentication Failed**: Verify your registry credentials
2. **Image Not Found**: Check if the image path is correct and exists in the registry
3. **Resource Limits**: Ensure your subscription has enough quota for the requested resources

For more help, check Azure Container Instances documentation or open an issue in this repository.
