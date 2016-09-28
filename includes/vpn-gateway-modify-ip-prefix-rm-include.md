### <a name="noconnection"></a>Voorvoegsels toevoegen of verwijderen - geen gatewayverbinding

- Gebruik het onderstaande voorbeeld om aanvullende adresvoorvoegsels **toe te voegen** aan een lokale netwerkgateway die u hebt gemaakt, maar die nog geen gatewayverbinding heeft. Wijzig de waarden in die van uzelf.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

- Gebruik het onderstaande voorbeeld om een adresvoorvoegsel **te verwijderen** uit een lokale netwerkgateway die geen VPN-verbinding heeft. Laat de voorvoegsels weg die u niet langer nodig hebt. In dit voorbeeld hebben we voorvoegsel 20.0.0.0/24 (van het vorige voorbeeld) niet langer nodig, dus werken we de lokale netwerkgateway bij zonder dit voorvoegsel.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')

### <a name="withconnection"></a>Voorvoegsels toevoegen of verwijderen - bestaande gatewayverbinding

Als u uw gatewayverbinding hebt gemaakt en u wilt IP-adresvoorvoegsels toevoegen aan of verwijderen uit uw lokale netwerkgateway, moet u de volgende stappen uitvoeren in de volgorde waarin ze staan vermeld. Dit veroorzaakt enige downtime in uw VPN-verbinding. Wanneer u uw voorvoegsels bijwerkt, moet u eerst de verbinding verwijderen, de voorvoegsels wijzigen, en vervolgens een nieuwe verbinding maken. Zorg dat u de waarden in de onderstaande voorbeelden door uw eigen waarden vervangt.

>[AZURE.IMPORTANT] Verwijder de VPN-gateway niet. Als u dat doet, moet u de stappen herhalen om hem opnieuw tot stand te brengen. Bovendien moet u uw on-premises router opnieuw configureren met de nieuwe instellingen.
 
1. Verwijder de verbinding.

        Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName

2. Wijzig de adresvoorvoegsels voor uw lokale netwerkgateway.

    Stel de variabele in voor LocalNetworkGateway.

        $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName

    Wijzig de voorvoegsels.

        Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
        -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')

4. Maak de verbinding. In dit voorbeeld configureren we een IPsec-verbindingstype. Wanneer u uw verbinding opnieuw maakt, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.

    Stel de variabele in voor VirtualNetworkGateway.

        $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName

    Maak de verbinding. Houd er rekening mee dat in dit voorbeeld de variabele $local wordt gebruikt die u in de vorige stap hebt ingesteld.


        New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
        -ResourceGroupName MyRGName -Location 'West US' `
        -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
        -ConnectionType IPsec `
        -RoutingWeight 10 -SharedKey 'abc123'


<!--HONumber=Sep16_HO3-->


