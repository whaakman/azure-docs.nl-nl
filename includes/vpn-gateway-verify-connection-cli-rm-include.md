U kunt controleren of uw verbinding is geslaagd met behulp van de opdracht [az network vpn-connection show](/cli/azure/network/vpn-connection#show). Configureer de waarden zodanig dat ze overeenkomen met uw eigen instellingen. In het voorbeeld verwijst '-name' naar de naam van de verbinding die u hebt gemaakt en die u wilt testen.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

Zolang het tot stand brengen van de verbinding nog bezig is, is de verbindingsstatus 'Verbinding maken'. Wanneer de verbinding tot stand is gebracht, verandert de status in 'Verbonden' en kunt u inkomende en uitgaande bytes zien.