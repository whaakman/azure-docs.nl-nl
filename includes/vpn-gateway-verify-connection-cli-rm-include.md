Met behulp van de volgende CLI-opdracht kunt u controleren of uw verbinding is geslaagd. Configureer de waarden zodanig dat ze overeenkomen met uw eigen instellingen. In het voorbeeld verwijst -n naar de naam van de verbinding die u hebt gemaakt en wilt testen.

```azurecli
az network vpn-connection show -n VNet1toSite2 -g TestRG1
```

Zolang het tot stand brengen van de verbinding nog bezig is, is de verbindingsstatus 'Verbinding maken'. Wanneer de verbinding tot stand is gebracht, verandert de status in 'Verbonden' en kunt u inkomende en uitgaande bytes zien.