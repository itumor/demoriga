
## Create a Kubernetes secret
The provider requires credentials to create and manage Azure resources.

### Install the Azure command-line
Generating an [authentication file](https://docs.microsoft.com/en-us/azure/developer/go/azure-sdk-authorization#use-file-based-authentication) requires the Azure command-line. Follow the documentation from Microsoft to [Download and install the Azure command-line](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

### Create an Azure service principal
Follow the Azure documentation to [find your Subscription ID](https://docs.microsoft.com/en-us/azure/azure-portal/get-subscription-tenant-id) from the Azure Portal.

Log in to the Azure command-line.

```command
az login
```

Using the Azure command-line and provide your Subscription ID create a service principal and authentication file.

```command
az ad sp create-for-rbac --sdk-auth --role Owner --scopes /subscriptions/<Subscription ID> 
```

The command generates a JSON file like this:
```json
{
  "clientId": "5d73973c-1933-4621-9f6a-9642db949768",
  "clientSecret": "24O8Q~db2DFJ123MBpB25hdESvV3Zy8bfeGYGcSd",
  "subscriptionId": "c02e2b27-21ef-48e3-96b9-a91305e9e010",
  "tenantId": "7060afec-1db7-4b6f-a44f-82c9c6d8762a",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Save this output as `azure-credentials.json`.

### Create a Kubernetes secret with the Azure credentials JSON file
Use `kubectl create secret -n upbound-system` to generate the Kubernetes secret object inside the Kubernetes cluster.

`kubectl create secret generic azure-secret -n upbound-system --from-file=creds=./azure-credentials.json`

View the secret with `kubectl describe secret`
```shell
$ kubectl describe secret azure-secret -n upbound-system
Name:         azure-secret
Namespace:    upbound-system
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
creds:  629 bytes
```