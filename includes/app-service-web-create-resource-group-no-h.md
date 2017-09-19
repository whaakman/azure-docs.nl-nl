Maak een resourcegroep in de Cloud Shell met de opdracht [az group create](/cli/azure/group#create).

[!INCLUDE [resource group intro text](resource-group.md)]

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *West-Europa*.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt. Als u alle ondersteunde locaties voor Azure Web Apps wilt zien, voert u de opdracht `az appservice list-locations` uit. 