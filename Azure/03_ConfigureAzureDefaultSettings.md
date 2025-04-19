# Configure Azure Default Settings

You can set the defaults for the current user's `az` commands.

Set default resource group and location:

```powershell
az config set defaults.group=rg-test
az config set defaults.location=newzealandnorth
```

## List all Resource Groups

List all the resource groups in the current subscriptions.
```powershell
az group list --query '[].name' -o tsv
```

List all the resource groups with details:
```powershell
az group list
```

## List all the Location

List all the Azure locations:
```powershell
az account list-locations -o table
```
