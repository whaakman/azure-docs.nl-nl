## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources, zoals functie-apps, databases en opslagaccounts worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt.  
Als u Cloud Shell niet gebruikt, moet u zich eerst aanmelden met `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt. Als u alle ondersteunde locaties voor App Service-abonnementen wilt zien, voert u de opdracht [az appservice list-locations](/cli/azure/appservice#az_appservice_list_locations) uit.