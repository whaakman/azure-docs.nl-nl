## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Functies maakt gebruik van een algemene account in Azure Storage te onderhouden, status en andere informatie over uw functies. Een algemeen opslagaccount maken in de resourcegroep die u hebt gemaakt met behulp van de [az storage-account maken](/cli/azure/storage/account#create) opdracht.

In de volgende opdracht te vervangen door een globally unique identifier opslagaccountnaam waarin u zien hoe de `<storage_name>` tijdelijke aanduiding. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Nadat het opslagaccount is gemaakt, toont Azure CLI soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```