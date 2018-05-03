Een resourcegroep maken met de opdracht [az group create](/cli/azure/group?view=azure-cli-latest#az_group_create).

[!INCLUDE [resource group intro text](resource-group.md)]

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *Europa West*.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Voer de opdracht `az appservice list-locations` uit voor een overzicht van de beschikbare locaties. In het algemeen maakt u resources in een regio bij u in de buurt.
