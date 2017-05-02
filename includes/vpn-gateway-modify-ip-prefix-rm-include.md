### <a name="noconnection"></a>Voorvoegsels wijzigen - geen gatewayverbinding

- Ga als volgt te werk om aanvullende voorvoegsels toe te voegen:

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```

- Ga als volgt te werk om een adresvoorvoegsel te verwijderen:<br>
  Laat de voorvoegsels weg die u niet langer nodig hebt. In dit voorbeeld hebben we voorvoegsel 20.0.0.0/24 (uit het vorige voorbeeld) niet langer nodig, dus werken we de lokale netwerkgateway bij zonder dit voorvoegsel.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName `
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','30.0.0.0/24')
  ```

### <a name="withconnection"></a>Voorvoegsels wijzigen - bestaande gatewayverbinding
Als u een gatewayverbinding hebt en u IP-adresvoorvoegsels wilt toevoegen aan of verwijderen uit uw lokale netwerkgateway, moet u de volgende stappen uitvoeren in de volgorde waarin ze staan vermeld. Dit veroorzaakt enige downtime in uw VPN-verbinding.

> [!IMPORTANT]
> Verwijder de VPN-gateway niet. Als u dit wel doet, moet u de stappen nogmaals doorlopen om deze opnieuw te maken. Bovendien moet u uw on-premises VPN-apparaat bijwerken met het nieuwe IP-adres van de VPN-gateway.
> 
> 

1. Verwijder de verbinding.

  ```powershell
  Remove-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName -ResourceGroupName MyRGName
  ```
2. Wijzig de adresvoorvoegsels voor uw lokale netwerkgateway.
   
  Stel de variabele in voor LocalNetworkGateway.

  ```powershell
  $local = Get-AzureRmLocalNetworkGateway -Name MyLocalNetworkGWName -ResourceGroupName MyRGName
  ```
   
  Wijzig de voorvoegsels.
   
  ```powershell
  Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $local `
  -AddressPrefix @('10.0.0.0/24','20.0.0.0/24','30.0.0.0/24')
  ```
3. Maak de verbinding. In dit voorbeeld configureren we een IPsec-verbindingstype. Wanneer u uw verbinding opnieuw maakt, gebruikt u het verbindingstype dat is opgegeven voor uw configuratie. Zie de pagina [PowerShell-cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) voor aanvullende verbindingstypen.
   
  Stel de variabele in voor VirtualNetworkGateway.

  ```powershell
  $gateway1 = Get-AzureRmVirtualNetworkGateway -Name RMGateway  -ResourceGroupName MyRGName
  ```
   
  Maak de verbinding. In dit voorbeeld wordt de variabele $local gebruikt die u in stap 2 hebt ingesteld.

  ```powershell
  New-AzureRmVirtualNetworkGatewayConnection -Name MyGWConnectionName `
  -ResourceGroupName MyRGName -Location 'West US' `
  -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
  -ConnectionType IPsec `
  -RoutingWeight 10 -SharedKey 'abc123'
  ```