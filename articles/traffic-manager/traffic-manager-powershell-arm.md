---
title: Traffic Manager in Azure beheren met behulp van PowerShell | Microsoft Docs
description: Met behulp van PowerShell voor Traffic Manager met Azure Resource Manager
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 1cd7bd7e32c96398d72c7cd3b51e2b456d60f01d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-powershell-to-manage-traffic-manager"></a>Traffic Manager beheren met behulp van PowerShell

Azure Resource Manager is de interface voorkeursbeheerpunten voor services in Azure. Azure Traffic Manager-profielen kunnen worden beheerd met Azure Resource Manager gebaseerde API's en hulpprogramma's.

## <a name="resource-model"></a>Resourcemodel

Azure Traffic Manager is geconfigureerd met behulp van een verzameling instellingen voor een Traffic Manager-profiel wordt aangeroepen. Dit profiel bevat DNS-instellingen, instellingen voor verkeersroutering, instellingen voor eindpuntcontrole en een lijst met de service-eindpunten waarop het verkeer wordt doorgestuurd.

Elke Traffic Manager-profiel wordt vertegenwoordigd door een resource van het type 'TrafficManagerProfiles'. Op het niveau van de REST-API is de URI voor elk profiel als volgt:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Instellen van Azure PowerShell

Deze instructies Microsoft Azure PowerShell gebruiken. Het volgende artikel wordt uitgelegd hoe u Azure PowerShell installeren en configureren.

* [Azure PowerShell installeren en configureren](/powershell/azure/overview)

De voorbeelden in dit artikel wordt ervan uitgegaan dat u een bestaande resourcegroep hebt. Kunt u een resourcegroep met de volgende opdracht:

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Resource Manager vereist dat alle resourcegroepen een locatie. Deze locatie wordt als standaardwaarde gebruikt voor resources in die resourcegroep hebt gemaakt. Echter, aangezien Traffic Manager-profiel-resources globaal en niet regionaal zijn, de keuze van de locatie voor resourcegroep heeft geen invloed op Azure Traffic Manager.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

U kunt een Traffic Manager-profiel maken de `New-AzureRmTrafficManagerProfile` cmdlet:

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

De volgende tabel beschrijft de parameters:

| Parameter | Beschrijving |
| --- | --- |
| Naam |De resourcenaam voor de resource Traffic Manager-profiel. Profielen in dezelfde resourcegroep moeten unieke namen hebben. Deze naam is gescheiden van de DNS-naam gebruikt voor DNS-query's. |
| resourceGroupName |De naam van de resourcegroep met de bron van het profiel. |
| TrafficRoutingMethod |Hiermee geeft u de verkeersroutering methode die wordt gebruikt om te bepalen welk eindpunt wordt geretourneerd als antwoord een DNS-query. Mogelijke waarden zijn 'Prestaties', 'Gewogen' of 'Prioriteit'. |
| RelativeDnsName |Hiermee geeft u het gedeelte hostname van de DNS-naam op die door dit Traffic Manager-profiel. Deze waarde wordt gecombineerd met de DNS-domeinnaam die door Azure Traffic Manager gebruikt voor het maken van de volledig gekwalificeerde domeinnaam (FQDN) van het profiel. De waarde van 'contoso' wordt bijvoorbeeld 'contoso.trafficmanager.net'. |
| TTL |Hiermee geeft u de DNS-Time-to-Live (TTL), in seconden. Deze TTL informeert de lokale DNS-resolvers en DNS-clients hoe lang cache DNS-antwoorden voor dit Traffic Manager-profiel. |
| MonitorProtocol |Hiermee geeft u het protocol te gebruiken om te controleren van endpoint-status. Mogelijke waarden zijn 'HTTP' en 'HTTPS'. |
| MonitorPort |Hiermee geeft u de TCP-poort gebruikt voor het controleren van endpoint-status. |
| MonitorPath |Hiermee geeft u het pad ten opzichte van de endpoint-domeinnaam die is gebruikt voor de probe voor eindpunt health. |

De cmdlet een Traffic Manager-profiel maakt in Azure en een bijbehorende profiel-object geretourneerd in PowerShell. Het profiel bevat geen eindpunten op dit moment geen. Zie voor meer informatie over het toevoegen van eindpunten aan een Traffic Manager-profiel [Traffic Manager-eindpunten toe te voegen](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Een Traffic Manager-profiel ophalen

Gebruik voor het ophalen van een bestaand Traffic Manager-profiel object, de `Get-AzureRmTrafficManagerProfle` cmdlet:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Deze cmdlet retourneert het object van een Traffic Manager-profiel.

## <a name="update-a-traffic-manager-profile"></a>Een Traffic Manager-profiel bijwerken

Wijzigen van Traffic Manager-profielen, volgt een 3-proces:

1. Ophalen van het profiel met behulp van `Get-AzureRmTrafficManagerProfile` of gebruik het profiel dat is geretourneerd door `New-AzureRmTrafficManagerProfile`.
2. Wijzig het profiel. U kunt toevoegen en verwijderen van eindpunten of wijzigt u de parameters voor het eindpunt of profiel. Deze wijzigingen zijn offline bewerkingen. Alleen wijzigt u het lokale object in het geheugen dat het profiel vertegenwoordigt.
3. Uw wijzigingen met behulp van de `Set-AzureRmTrafficManagerProfile` cmdlet.

Alle eigenschappen van het profiel kunnen worden gewijzigd, met uitzondering van het profiel RelativeDnsName. Als u wilt de RelativeDnsName wijzigen, moet u profiel en een nieuw profiel met een nieuwe naam verwijderen.

Het volgende voorbeeld laat zien hoe de TTL van het profiel te wijzigen:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Er zijn drie soorten Traffic Manager-eindpunten:

1. **Azure-eindpunten** zijn services die worden gehost in Azure
2. **Externe eindpunten** worden services die buiten Azure worden gehost
3. **Genest eindpunten** worden gebruikt voor geneste hiërarchieën van Traffic Manager-profielen. Geneste eindpunten inschakelen geavanceerde verkeersroutering configuraties voor complexe toepassingen.

In alle drie gevallen kunnen eindpunten worden toegevoegd op twee manieren:

1. Met behulp van een proces 3-stappen die hierboven is beschreven. Het voordeel van deze methode is dat enkele eindpunt kan worden gewijzigd in één update.
2. Met behulp van de cmdlet New-AzureRmTrafficManagerEndpoint. Deze cmdlet voegt een eindpunt toe aan een bestaand Traffic Manager-profiel in één bewerking.

## <a name="adding-azure-endpoints"></a>Azure-eindpunten toevoegen

Azure-eindpunten verwijzen naar services die worden gehost in Azure. Twee soorten Azure-eindpunten worden ondersteund:

1. Azure Web Apps
2. Azure PublicIpAddress-resources (die kunnen worden gekoppeld aan een load balancer of een virtuele machine NIC). De PublicIpAddress moet een DNS-naam toegewezen aan het in Traffic Manager worden gebruikt.

In elk geval:

* De service wordt opgegeven met de parameter 'targetResourceId' van `Add-AzureRmTrafficManagerEndpointConfig` of `New-AzureRmTrafficManagerEndpoint`.
* De 'Target' en 'EndpointLocation' zijn door de TargetResourceId ingesloten.
* Geven de 'gewicht', is optioneel. Gewicht worden alleen gebruikt als het profiel is geconfigureerd voor gebruik van de methode 'Gewogen' verkeer routering. Anders worden genegeerd. Indien opgegeven, is de waarde moet een getal tussen 1 en 1000. De standaardwaarde is '1'.
* Geven de 'prioriteit' is optioneel. Prioriteiten worden alleen gebruikt als het profiel is geconfigureerd voor gebruik van de methode 'Prioriteit' verkeer routering. Anders worden genegeerd. Geldige waarden liggen tussen 1 en 1000 met lagere waarden die wijzen op een hogere prioriteit. Als voor één eindpunt opgegeven, moeten ze worden opgegeven voor alle eindpunten. Als u niets opgeeft, worden in de volgorde waarin de eindpunten zijn weergegeven standaardwaarden '1' vanaf toegepast.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Voorbeeld 1: Toe te voegen met behulp van Web-App-eindpunten`Add-AzureRmTrafficManagerEndpointConfig`

In dit voorbeeld wordt een Traffic Manager-profiel maken en toevoegen van eindpunten van Web-App, met behulp van de `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Toevoegen van een publicIpAddress-eindpunt met`New-AzureRmTrafficManagerEndpoint`

In dit voorbeeld wordt de bron van een openbare IP-adres toegevoegd aan het Traffic Manager-profiel. Het openbare IP-adres moet een DNS-naam is geconfigureerd, en kan worden gebonden aan de NIC van een virtuele machine of aan een load balancer.

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Externe eindpunten toevoegen

Externe eindpunten Traffic Manager gebruikt voor het verkeer naar de services die buiten Azure worden gehost. Als u met Azure-eindpunten, externe eindpunten kunnen worden toegevoegd, met behulp van `Add-AzureRmTrafficManagerEndpointConfig` gevolgd door `Set-AzureRmTrafficManagerProfile`, of `New-AzureRMTrafficManagerEndpoint`.

Wanneer u externe eindpunten opgeeft:

* De naam van het domein moet worden opgegeven met de parameter 'Target'
* Als de methode 'Prestaties' verkeer routering wordt gebruikt, wordt de 'EndpointLocation' is vereist. Anders is optioneel. De waarde moet een [geldig Azure-regionaam](https://azure.microsoft.com/regions/).
* Het 'Gewicht' en 'Prioriteit' zijn optioneel.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Externe eindpunten met toevoegen `Add-AzureRmTrafficManagerEndpointConfig` en`Set-AzureRmTrafficManagerProfile`

In dit voorbeeld wordt een Traffic Manager-profiel maken, twee externe eindpunten toevoegen en de wijzigingen worden doorgevoerd.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Externe eindpunten met toevoegen`New-AzureRmTrafficManagerEndpoint`

In dit voorbeeld wordt een extern eindpunt toevoegen aan een bestaand profiel. Het profiel is opgegeven met behulp van de groepnamen van profielen en resource.

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>'Nested' eindpunten toevoegen

Elke Traffic Manager-profiel geeft één verkeersroutering methode. Er zijn echter scenario's waarvoor meer geavanceerde voor verkeersroutering dan de routering geleverd door een enkele Traffic Manager-profiel. U kunt de Traffic Manager-profielen voor het combineren van de voordelen van meer dan één methode voor verkeersroutering nesten. Geneste profielen kunnen u het standaardgedrag van Traffic Manager ondersteuning grotere en complexere implementaties van toepassingen vervangen. Zie voor meer voorbeelden, [genest Traffic Manager-profielen](traffic-manager-nested-profiles.md).

Geneste eindpunten zijn geconfigureerd op het bovenliggende-profiel met een specifieke eindpunt-type 'NestedEndpoints'. Bij het opgeven van geneste eindpunten:

* Het eindpunt moet worden opgegeven met de parameter 'targetResourceId'
* Als de methode 'Prestaties' verkeer routering wordt gebruikt, wordt de 'EndpointLocation' is vereist. Anders is optioneel. De waarde moet een [geldig Azure-regionaam](http://azure.microsoft.com/regions/).
* De 'Gewicht' en 'Prioriteit' zijn optioneel, als voor de Azure-eindpunten.
* De parameter 'MinChildEndpoints' is optioneel. De standaardwaarde is '1'. Als het aantal beschikbare eindpunten kleiner is dan deze drempelwaarde, wordt het profiel van de bovenliggende beschouwt het onderliggende profiel 'Gedegradeerd' en verkeer naar de andere eindpunten in het profiel van de bovenliggende zorgt.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Toe te voegen geneste eindpunten met `Add-AzureRmTrafficManagerEndpointConfig` en`Set-AzureRmTrafficManagerProfile`

In dit voorbeeld we nieuwe Traffic Manager onderliggende en bovenliggende profielen maken, het onderliggende toevoegen als een geneste eindpunt voor de bovenliggende en de wijzigingen worden doorgevoerd.

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Voor beknopt alternatief bevat in dit voorbeeld heeft we geen andere eindpunten niet toevoegen aan de onderliggende of bovenliggende profielen.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Geneste eindpunten toevoegen`New-AzureRmTrafficManagerEndpoint`

In dit voorbeeld toevoegen we een bestaand profiel van de onderliggende als een geneste eindpunt aan een bestaand profiel van de bovenliggende. Het profiel is opgegeven met behulp van de groepnamen van profielen en resource.

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a>Bijwerken van een Traffic Manager-eindpunt

Er zijn twee manieren om bij te werken van een bestaand Traffic Manager-eindpunt:

1. Ophalen van het Traffic Manager via `Get-AzureRmTrafficManagerProfile`, de eigenschappen van de endpoint binnen het profiel bijwerken en de wijzigingen met behulp van `Set-AzureRmTrafficManagerProfile`. Deze methode heeft het voordeel van meer dan één eindpunt in één bewerking worden bijgewerkt.
2. Ophalen van het Traffic Manager-eindpunt met behulp `Get-AzureRmTrafficManagerEndpoint`, de eindpunt-eigenschappen niet bijwerken, en de wijzigingen met behulp van `Set-AzureRmTrafficManagerEndpoint`. Deze methode is eenvoudiger, aangezien hoeven niet te indexeren bij de matrix eindpunten in het profiel.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Bijwerken met behulp van eindpunten `Get-AzureRmTrafficManagerProfile` en`Set-AzureRmTrafficManagerProfile`

In dit voorbeeld wijzigt u de prioriteit op twee eindpunten in een bestaand profiel.

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Bijwerken van een eindpunt met `Get-AzureRmTrafficManagerEndpoint` en`Set-AzureRmTrafficManagerEndpoint`

In dit voorbeeld wijzigen we het gewicht van één eindpunt in een bestaand profiel.

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Eindpunten en -profielen inschakelen en uitschakelen

Traffic Manager kunt afzonderlijke eindpunten zijn ingeschakeld en wordt uitgeschakeld, evenals inschakelen en uitschakelen van volledige profielen toestaan.
Deze wijzigingen kunnen de bronnen eindpunt of profiel door ophalen/bijwerken/instelling aangebracht. Deze algemene bewerkingen stroomlijnen, worden ze ook ondersteund via exclusieve cmdlets.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Voorbeeld 1: Inschakelen en uitschakelen van een Traffic Manager-profiel

Gebruiken om een Traffic Manager-profiel, `Enable-AzureRmTrafficManagerProfile`. Het profiel kan worden opgegeven met behulp van een object van een profiel. Een object van het profiel kan worden doorgegeven via de pijplijn of met behulp van de '-TrafficManagerProfile' parameter. In dit voorbeeld Geef we het profiel met de naam van de groep profiel en de resource.

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Een Traffic Manager-profiel uitschakelen:

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

De cmdlet Disable-AzureRmTrafficManagerProfile vraagt om bevestiging. Dit bericht kan worden onderdrukt met behulp van de '-Force' parameter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Voorbeeld 2: Inschakelen en uitschakelen van een Traffic Manager-eindpunt

Gebruiken om een Traffic Manager-eindpunt, `Enable-AzureRmTrafficManagerEndpoint`. Er zijn twee manieren om op te geven van het eindpunt

1. Een TrafficManagerEndpoint-object doorgegeven via de pijplijn gebruiken of met behulp van de '-TrafficManagerEndpoint' parameter
2. Met behulp van de naam van het eindpunt, eindpunttype, profielnaam en de naam van resourcegroep:

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Op deze manier een Traffic Manager-eindpunt uitschakelen:

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Net als bij `Disable-AzureRmTrafficManagerProfile`, wordt de `Disable-AzureRmTrafficManagerEndpoint` cmdlet vraagt om bevestiging. Dit bericht kan worden onderdrukt met behulp van de '-Force' parameter.

## <a name="delete-a-traffic-manager-endpoint"></a>Een Traffic Manager-eindpunt verwijderen

U kunt afzonderlijke eindpunten verwijderen met de `Remove-AzureRmTrafficManagerEndpoint` cmdlet:

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Deze cmdlet vraagt om bevestiging. Dit bericht kan worden onderdrukt met behulp van de '-Force' parameter.

## <a name="delete-a-traffic-manager-profile"></a>Een Traffic Manager-profiel verwijderen

Als u wilt een Traffic Manager-profiel verwijdert, gebruikt u de `Remove-AzureRmTrafficManagerProfile` geven de groepnamen van profielen en resource-cmdlet:

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Deze cmdlet vraagt om bevestiging. Dit bericht kan worden onderdrukt met behulp van de '-Force' parameter.

Het profiel te verwijderen kan ook worden opgegeven met behulp van een object van een profiel:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Deze reeks kan ook worden doorgesluisd:

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Volgende stappen

[Traffic Manager-controle](traffic-manager-monitoring.md)

[Prestatieoverwegingen voor Traffic Manager](traffic-manager-performance-considerations.md)
