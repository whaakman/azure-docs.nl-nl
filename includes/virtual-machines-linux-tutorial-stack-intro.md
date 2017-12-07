## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een VM met de opdracht [az vm create](/cli/azure/vm#az_vm_create). 

In het volgende voorbeeld wordt een VM gemaakt met de naam *myVM* en worden er SSH-sleutels gemaakt, als deze nog niet bestaan op een standaardsleutellocatie. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. De opdracht stelt ook *azureuser* als de gebruikersnaam van een beheerder. U gebruikt deze naam later verbinding maken met de virtuele machine. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Wanneer de virtuele machine is gemaakt, toont de Azure CLI informatie die lijkt op de informatie in het volgende voorbeeld. Noteer het `publicIpAddress`. Dit adres wordt gebruikt voor toegang tot de virtuele machine in latere stappen.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```



## <a name="open-port-80-for-web-traffic"></a>Poort 80 openen voor webverkeer 

Standaard worden alleen SSH-verbindingen zijn toegestaan in Linux VM's geïmplementeerd in Azure. Omdat deze virtuele machine is het verstandig om een webserver, moet u poort 80 openen vanuit het internet. Gebruik de opdracht [az vm open-port](/cli/azure/vm#az_vm_open_port) om de gewenste poort te openen.  
 
```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```
## <a name="ssh-into-your-vm"></a>SSH in uw virtuele machine


Als u al het openbare IP-adres van uw virtuele machine niet weet, voert u de [az netwerk openbare ip-lijst](/cli/azure/network/public-ip#list) opdracht. U moet dit IP-adres voor de verschillende volgende stappen.


```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

Gebruik de volgende opdracht om een SSH-sessie te starten voor de virtuele machine. Vervangen door de juiste openbare IP-adres van uw virtuele machine. In dit voorbeeld wordt het IP-adres is *40.68.254.142*. *azureuser* is de naam van de administrator-gebruiker ingesteld tijdens het maken van de virtuele machine.

```bash
ssh azureuser@40.68.254.142
```

