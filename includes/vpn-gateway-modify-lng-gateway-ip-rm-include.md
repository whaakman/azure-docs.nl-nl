Gebruik de cmdlet `New-AzureRmVirtualNetworkGatewayConnection` om het IP-adres van de gateway te wijzigen. Zolang u de naam van de gateway van het lokale netwerk precies hetzelfde houdt als de bestaande naam, worden de instellingen overschreven. Op dit moment biedt de cmdlet Set geen ondersteuning voor het wijzigen van het IP-adres van de gateway.

### <a name="gwipnoconnection"></a>Het IP-adres van de gateway wijzigen - geen gatewayverbinding
Gebruik het onderstaande voorbeeld om het IP-adres bij te werken van de gateway in uw lokale netwerk dat nog geen verbinding heeft. U kunt tegelijk ook de adresvoorvoegsels bijwerken. De instellingen die u opgeeft, zorgen ervoor dat de bestaande instellingen worden overschreven. Zorg ervoor dat u de bestaande naam van de gateway van uw lokale netwerk gebruikt. Als u dit niet doet, maakt u een nieuwe lokale netwerkgateway, die de bestaande gateway niet overschrijft.

Gebruik het volgende voorbeeld en vervang daarin de waarden door uw eigen waarden.

    New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
    -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
    -GatewayIpAddress "5.4.3.2" -ResourceGroupName MyRGName


### <a name="gwipwithconnection"></a>Het IP-adres van de gateway wijzigen - bestaande gatewayverbinding
Als er al een gatewayverbinding bestaat, moet u die verbinding eerst verwijderen. Vervolgens kunt u het IP-adres van de gateway wijzigen en een nieuwe verbinding maken. Dit veroorzaakt enige downtime in uw VPN-verbinding.

> [!IMPORTANT]
> Verwijder de VPN-gateway niet. Als u dit doet, moet u in de stappen teruggaan om een nieuwe te maken, en ook uw on-premises router opnieuw configureren met het IP-adres dat wordt toegewezen aan de nieuwe gateway.
> 
> 

1. Verwijder de verbinding. U vindt de naam van de verbinding met behulp van de cmdlet `Get-AzureRmVirtualNetworkGatewayConnection`.
   
        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName
2. De GatewayIpAddress-waarde wijzigen. U kunt nu indien nodig ook uw adresvoorvoegsels wijzigen. Houd er rekening mee dat deze de bestaande instellingen voor de gateway van het lokale netwerk overschrijven. Gebruik de bestaande naam van de gateway van uw lokale netwerk bij het wijzigen, zodat de instellingen worden overschreven. Als u dit niet doet, maakt u een nieuwe gateway voor uw lokale netwerk en blijft de bestaande gateway ongewijzigd.
   
        New-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName `
        -Location "West US" -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24') `
        -GatewayIpAddress "104.40.81.124" -ResourceGroupName MyRGName
3. Maak de verbinding. In dit voorbeeld configureren we een IPsec-verbindingstype. Wanneer u uw verbinding opnieuw maakt, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.  U kunt de cmdlet `Get-AzureRmVirtualNetworkGateway` uitvoeren om de VirtualNetworkGateway-naam te verkrijgen.
   
    Stel de variabelen in:
   
        $local = Get-AzureRMLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        $vnetgw = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName MyRGName
   
    Maak de verbinding:
   
        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName `
        -Location "West US" `
        -VirtualNetworkGateway1 $vnetgw `
        -LocalNetworkGateway2 $local `
        -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

<!--HONumber=Sep16_HO3-->


