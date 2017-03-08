## <a name="prerequisites"></a>Vereisten

Als u dat nog niet hebt gedaan, vraag dan een [gratis proefabonnement van Azure aan](https://azure.microsoft.com/pricing/free-trial/) en installeer de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="create-the-scale-set"></a>De schaalset maken

Maak eerst een resourcegroep om de schaalset in te implementeren:

```azurecli
az group create --location westus --name myResourceGroup
```

Maak nu de schaalset met behulp van de opdracht `az vmss create`. In het volgende voorbeeld wordt een Linux-schaalset gemaakt genaamd `myvmss` in de resourcegroep met de naam `myrg`:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image UbuntuLTS --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

In het volgende voorbeeld wordt een Windows-schaalset gemaakt met dezelfde configuratie:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image Win2016Datacenter --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Als u een andere installatiekopie van het besturingssysteem wilt kiezen, kunt u de beschikbare installatiekopieën bekijken met opdracht `az vm image list` of `az vm image list --all`. Als u de verbindingsgegevens van de virtuele machines in de schaalset wilt bekijken, gebruikt u de opdracht `az vmss list_instance_connection_info`:

```azurecli
az vmss list_instance_connection_info --resource-group myResourceGroup --name myVmss
```