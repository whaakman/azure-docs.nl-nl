### <a name="to-verify-your-connection-by-using-powershell"></a>Uw verbinding controleren met behulp van PowerShell

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

### <a name="to-verify-your-connection-by-using-the-azure-portal"></a>Uw verbinding controleren met behulp van de Azure-portal

U kunt de verbindingsstatus weergeven door naar de verbinding te navigeren in de Azure-portal. Er zijn meerdere manieren om dit te doen. De volgende stappen tonen één manier om te navigeren naar uw verbinding en deze te controleren.

1. Klik in [Azure Portal](http://portal.azure.com) op **Alle resources** en navigeer naar de virtuele netwerkgateway.
2. Klik op de blade voor uw virtuele netwerkgateway op **Verbindingen**. U ziet de status van elke verbinding.
3. Klik op de naam van de verbinding die u wilt controleren om **Essentials** te openen. In Essentials kunt u meer informatie weergeven over de verbinding. Wanneer u verbinding hebt gemaakt, is de **status** Geslaagd en Verbonden.

    ![Verbinding controleren](./media/vpn-gateway-verify-connection-rm-include/connectionsucceeded.png)

<!--HONumber=Oct16_HO3-->


