U moet een VNet en een gatewaysubnet eerst maken voordat u gaat werken op de volgende taken.

> [!NOTE]
> Deze voorbeelden gelden niet voor S2S en ExpressRoute configuraties naast elkaar bestaan.
> Zie voor meer informatie over het werken met gateways in een configuratie gebruiken [naast elkaar bestaande verbindingen configureren](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Een gateway toevoegen

Gebruik de onderstaande opdracht voor het maken van een gateway. Zorg ervoor dat vervangen door uw eigen waarden.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Controleer of dat de gateway is gemaakt

Gebruik de onderstaande opdracht om te controleren of de gateway is aangemaakt. Met deze opdracht haalt ook de gateway-ID die u nodig hebt voor andere bewerkingen.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Een gateway vergroten of verkleinen

Er zijn een aantal [Gateway-SKU's](../articles/expressroute/expressroute-about-virtual-network-gateways.md). De volgende opdracht kunt u de Gateway-SKU op elk moment wijzigen.

> [!IMPORTANT]
> Met deze opdracht werkt niet voor UltraPerformance gateway. Als u wilt uw gateway uit naar een gateway UltraPerformance wijzigen, verwijdert u eerst de bestaande ExpressRoute-gateway en maak vervolgens een nieuwe UltraPerformance-gateway. Als u wilt uw gateway van een gateway UltraPerformance downgraden, verwijdert u eerst de gateway UltraPerformance en maak vervolgens een nieuwe gateway. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Een gateway verwijderen

Gebruik de onderstaande opdracht om een gateway te verwijderen

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```