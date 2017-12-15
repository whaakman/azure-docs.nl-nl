Maak een resourcegroep in de Cloud Shell met de opdracht [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create).

[!INCLUDE [resource group intro text](resource-group.md)]

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *West-Europa*. Als u wilt zien alle ondersteunde locaties voor App Service, de `az appservice list-locations` opdracht.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt. 
