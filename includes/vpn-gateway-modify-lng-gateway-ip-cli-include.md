### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>Het 'gatewayIpAddress' van de lokale netwerkgateway wijzigen

Als van het VPN-apparaat waarmee u verbinding wilt maken het openbare IP-adres is gewijzigd, moet u de gateway van het lokale netwerk aanpassen met deze wijziging. Het IP-adres van de gateway kan worden gewijzigd zonder een bestaande VPN-gatewayverbinding te verwijderen (als u er een hebt). Voor het wijzigen van het gateway-IP-adres vervang u de waarden 'Site2' en 'TestRG1' door uw eigen waarden met behulp van de opdracht [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#az_network_local_gateway_update).

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Controleer of het IP-adres juist is in de uitvoer:

```
"gatewayIpAddress": "23.99.222.170",
```