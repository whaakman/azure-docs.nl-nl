### Uw verbinding controleren met behulp van PowerShell

U kunt controleren of uw verbinding is geslaagd door de cmdlet `Get-AzureRmVirtualNetworkGatewayConnection` te gebruiken, met of zonder `-Debug`. 

1. Gebruik het volgende cmdlet-voorbeeld om de waarden aan te passen aan uw eigen waarden. Selecteer A als dit wordt gevraagd om alles uit te voeren. In het voorbeeld verwijst `-Name` naar de naam van de verbinding die u hebt gemaakt en die u wilt testen.

        Get-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnection -ResourceGroupName MyRG

2. Bekijk de waarden nadat de cmdlet is voltooid. In het onderstaande voorbeeld wordt de verbindingsstatus weergegeven als Verbonden en ziet u inkomende en uitgaande bytes.

        Body:
        {
          "name": "MyGWConnection",
          "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/connections/MyGWConnection",
          "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "1c484f82-23ec-47e2-8cd8-231107450446b",
            "virtualNetworkGateway1": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/virtualNetworkGa
        teways/vnetgw1"
            },
            "localNetworkGateway2": {
              "id":
        "/subscriptions/086cfaa0-0d1d-4b1c-94544-f8e3da2a0c7789/resourceGroups/MyRG/providers/Microsoft.Network/localNetworkGate
        ways/LocalSite"
            },
            "connectionType": "IPsec",
            "routingWeight": 10,
            "sharedKey": "abc123",
            "connectionStatus": "Connected",
            "ingressBytesTransferred": 33509044,
            "egressBytesTransferred": 4142431
          }

### Uw verbinding controleren met behulp van de Azure-portal

U kunt de verbindingsstatus weergeven door naar de verbinding te navigeren in de Azure-portal. Er zijn meerdere manieren om dit te doen. Hieronder volgt één manier om naar de verbinding te navigeren.

1. Ga in de [Azure-portal](http://portal.azure.com) naar **Virtuele netwerkgateways**. Klik op de naam van uw gateway.
2. Klik in het deelvenster onder **Instellingen** op **Verbindingen**. U ziet de status van elke verbinding.
3. Klik voor meer informatie over de verbinding op de naam van de verbinding. Bekijk de **Verbindingsstatus** op de pagina Essentials voor uw verbinding. De status is 'Geslaagd' en 'Verbonden' wanneer u verbinding hebt gemaakt. U kunt de gegevensstroom controleren door **Gegevens in** en **Gegevens uit** te bekijken.

    In het onderstaande voorbeeld is de **Verbindingsstatus** 'Niet verbonden'. 

    ![Verbinding controleren](./media/vpn-gateway-verify-connection-rm-include/connectionverify450.png)


<!--HONumber=Aug16_HO4-->


