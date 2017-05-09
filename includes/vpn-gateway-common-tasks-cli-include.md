### <a name="to-view-local-network-gateways"></a>Lokale netwerkgateways bekijken

Gebruik de opdracht [az network local-gateway list](https://docs.microsoft.com/cli/azure/network/local-gateway#list) om een lijst met gateways voor lokale netwerken weer te geven.

```azurecli
az network local-gateway list --resource-group TestRG1
```

[!INCLUDE [modify-prefix](vpn-gateway-modify-ip-prefix-cli-include.md)]

[!INCLUDE [modify-gateway-IP](vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

### <a name="to-verify-the-shared-key-values"></a>De gedeelde sleutelwaarden controleren

Controleer of de waarde van de gedeelde sleutel gelijk is aan de waarde die u voor de configuratie van uw VPN-apparaat hebt gebruikt. Als dat niet het geval is, voert u de verbinding opnieuw uit met de waarde van het apparaat of werkt u het apparaat bij met de waarde die is geretourneerd. De waarden moeten overeenkomen. Gebruik [az network vpn-connection-list](https://docs.microsoft.com/cli/azure/network/vpn-connection#list) om de gedeelde sleutel weer te geven.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 --resource-group TestRG1
```
### <a name="to-view-the-vpn-gateway-public-ip-address"></a>Het openbare IP-adres van de VPN Gateway weergeven

Gebruik de opdracht [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#list) om het openbare IP-adres van uw virtuele netwerkgateway te vinden. De lijst met openbare IP-adressen wordt in dit voorbeeld in een gemakkelijk leesbare tabelindeling weergegeven.

```azurecli
az network public-ip list --resource-group TestRG1 --output table
```
