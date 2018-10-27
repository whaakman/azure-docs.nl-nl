---
title: Met behulp van PowerShell voor het beheren van Traffic Manager in Azure | Microsoft Docs
description: Met behulp van PowerShell voor Traffic Manager met Azure Resource Manager
services: traffic-manager
documentationcenter: na
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 8dcd89415bdd48b2d8d5c8e1e699159e9d1129e5
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139472"
---
# <a name="using-powershell-to-manage-traffic-manager"></a>Met behulp van PowerShell voor het beheren van Traffic Manager

Azure Resource Manager is de interface voorkeursbeheerpunten voor services in Azure. Azure Traffic Manager-profielen kunnen worden beheerd met behulp van Azure Resource Manager gebaseerde API's en hulpprogramma's.

## <a name="resource-model"></a>Resourcemodel

Met Azure Traffic Manager is geconfigureerd met behulp van een verzameling instellingen met de naam een Traffic Manager-profiel. Dit profiel bevat DNS-instellingen, instellingen voor routering van verkeer eindpunt controle-instellingen en een lijst met service-eindpunten waarmee verkeer wordt doorgestuurd.

Elke Traffic Manager-profiel wordt vertegenwoordigd door een resource van het type 'TrafficManagerProfiles'. Op het niveau van de REST-API is de URI voor elk profiel als volgt:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Instellen van Azure PowerShell

Deze instructies gebruikt Microsoft Azure PowerShell. Het volgende artikel wordt uitgelegd hoe u Azure PowerShell installeren en configureren.

* [Azure PowerShell installeren en configureren](/powershell/azure/overview)

De voorbeelden in dit artikel wordt ervan uitgegaan dat u een bestaande resourcegroep hebt. U kunt maken van een resourcegroep met de volgende opdracht:

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Resource Manager vereist dat alle resourcegroepen een locatie. Deze locatie wordt als standaardwaarde gebruikt voor resources die zijn gemaakt in die resourcegroep. Echter, aangezien Traffic Manager-profiel-resources globaal en niet regionaal zijn, de keuze van de locatie voor resourcegroep heeft geen invloed op Azure Traffic Manager.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

U kunt een Traffic Manager-profiel maken met de `New-AzureRmTrafficManagerProfile` cmdlet:

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

De volgende tabel beschrijft de parameters:

| Parameter | Beschrijving |
| --- | --- |
| Naam |De resourcenaam voor de resource Traffic Manager-profiel. De profielen in dezelfde resourcegroep bevinden moeten unieke namen hebben. Deze naam staat los van de DNS-naam die wordt gebruikt voor DNS-query's. |
| ResourceGroupName |De naam van de resourcegroep met de profiel-resource. |
| TrafficRoutingMethod |Hiermee geeft u de routering van verkeer in-methode gebruikt om te bepalen welk eindpunt wordt geretourneerd in antwoord een DNS-query. Mogelijke waarden zijn 'Prestaties', 'Gewogen' of 'Prioriteit'. |
| RelativeDnsName |Hiermee geeft u het gedeelte van de hostnaam van de DNS-naam die is opgegeven door dit Traffic Manager-profiel. Deze waarde wordt gecombineerd met de DNS-naam van Azure Traffic Manager gebruikt om de volledig gekwalificeerde domeinnaam (FQDN) van het profiel. De waarde van 'contoso' wordt bijvoorbeeld 'contoso.trafficmanager.net'. |
| TTL |Hiermee geeft u de DNS-Time-to-Live (TTL), in seconden. Deze TTL informeert de lokale DNS-resolvers en DNS-clients hoelang de cache-DNS-antwoorden voor dit Traffic Manager-profiel. |
| MonitorProtocol |Hiermee geeft u het protocol moet worden gebruikt voor het bewaken van de gezondheid van het eindpunt. Mogelijke waarden zijn 'HTTP' en 'HTTPS'. |
| MonitorPort |Hiermee geeft u de TCP-poort gebruikt voor het bewaken van de gezondheid van het eindpunt. |
| MonitorPath |Hiermee geeft u het pad relatief ten opzichte van de eindpunt-domeinnaam die is gebruikt voor het eindpunt status-test. |

De cmdlet een Traffic Manager-profiel maakt in Azure en een bijbehorende profiel-object geretourneerd naar PowerShell. Op dit moment het vervangingsprofiel bevat geen eindpunten. Zie voor meer informatie over het toevoegen van eindpunten aan een Traffic Manager-profiel [Traffic Manager-eindpunten toe te voegen](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Een Traffic Manager-profiel ophalen

Als u wilt een bestaand Traffic Manager-profiel-object ophalen, gebruikt u de `Get-AzureRmTrafficManagerProfle` cmdlet:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Deze cmdlet retourneert een Traffic Manager-profiel.

## <a name="update-a-traffic-manager-profile"></a>Een Traffic Manager-profiel bijwerken

Wijzigen van Traffic Manager-profielen met een 3-proces de volgende:

1. Ophalen van het profiel met `Get-AzureRmTrafficManagerProfile` of gebruikt u het profiel dat is geretourneerd door `New-AzureRmTrafficManagerProfile`.
2. Wijzig het profiel. U kunt toevoegen en verwijderen van eindpunten of wijzigt u de parameters voor het eindpunt of een profiel. Deze wijzigingen zijn offline bewerkingen. Alleen wijzigt u het lokaal object in het geheugen dat het profiel vertegenwoordigt.
3. Uw wijzigingen met behulp van de `Set-AzureRmTrafficManagerProfile` cmdlet.

Alle eigenschappen van het profiel kunnen worden gewijzigd, met uitzondering van het profiel RelativeDnsName. Als u wilt de RelativeDnsName wijzigen, moet u profiel en een nieuw profiel met een nieuwe naam verwijderen.

Het volgende voorbeeld ziet u hoe u het profiel van de TTL wijzigen:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Er zijn drie typen van Traffic Manager-eindpunten:

1. **Azure-eindpunten** zijn services die worden gehost in Azure
2. **Externe eindpunten** zijn services die buiten Azure worden gehost
3. **Geneste eindpunten** worden gebruikt voor het geneste hiërarchieën van Traffic Manager-profielen. Geneste-eindpunten kunt geavanceerde routering van verkeer configuraties voor complexe toepassingen.

In alle drie gevallen kunnen eindpunten worden toegevoegd op twee manieren:

1. Met behulp van een 3-stappen die eerder zijn beschreven. Het voordeel van deze methode is dat verschillende eindpunt kan worden gewijzigd in één update.
2. Met behulp van de cmdlet New-AzureRmTrafficManagerEndpoint. Deze cmdlet wordt een eindpunt toegevoegd aan een bestaand Traffic Manager-profiel in één bewerking.

## <a name="adding-azure-endpoints"></a>Azure-eindpunten toevoegen

Azure-eindpunten verwijzen naar services die worden gehost in Azure. Twee typen Azure-eindpunten worden ondersteund:

1. Azure Web Apps
2. Azure PublicIpAddress-resources (die kunnen worden gekoppeld aan een load balancer of een virtuele machine NIC). Het openbare IP-adres moet een DNS-naam toegewezen aan het in Traffic Manager worden gebruikt.

In elk geval:

* De service is opgegeven met de parameter 'targetResourceId' van `Add-AzureRmTrafficManagerEndpointConfig` of `New-AzureRmTrafficManagerEndpoint`.
* De 'Target' en 'EndpointLocation' zijn impliciet door de TargetResourceId.
* Opgeven de 'gewicht' is optioneel. Het gewicht worden alleen gebruikt als het profiel is geconfigureerd voor het gebruik van de methode 'Gewogen' Routering van verkeer. Anders worden deze genegeerd. Als u opgeeft, is de waarde moet een getal tussen 1 en 1000. De standaardwaarde is '1'.
* Opgeven de 'prioriteit' is optioneel. Prioriteiten worden alleen gebruikt als het profiel is geconfigureerd voor het gebruik van de methode 'Prioriteit' Routering van verkeer. Anders worden deze genegeerd. Geldige waarden zijn tussen 1 en 1000 met lagere waarden die wijzen op een hogere prioriteit. Als voor één eindpunt opgegeven, moeten ze worden opgegeven voor alle eindpunten. Als u dit weglaat, wordt standaardwaarden, beginnend bij '1' worden toegepast in de volgorde waarin de eindpunten worden weergegeven.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Voorbeeld 1: Toe te voegen met behulp van Web-App-eindpunten `Add-AzureRmTrafficManagerEndpointConfig`

In dit voorbeeld wordt een Traffic Manager-profiel maken en toevoegen van twee Web-App-eindpunten met behulp van de `Add-AzureRmTrafficManagerEndpointConfig` cmdlet.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Toevoegen van een openbare IP-adres-eindpunt met `New-AzureRmTrafficManagerEndpoint`

In dit voorbeeld wordt een openbaar IP-adresresource toegevoegd aan de Traffic Manager-profiel. Het openbare IP-adres moet een DNS-naam die is geconfigureerd, en kan worden gebonden aan de NIC van een virtuele machine of aan een load balancer.

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Externe eindpunten toevoegen

Externe eindpunten Traffic Manager gebruikt om te leiden verkeer naar services die buiten Azure worden gehost. Als u met Azure-eindpunten, externe eindpunten kunnen worden toegevoegd met behulp van `Add-AzureRmTrafficManagerEndpointConfig` gevolgd door `Set-AzureRmTrafficManagerProfile`, of `New-AzureRMTrafficManagerEndpoint`.

Bij het opgeven van externe eindpunten:

* De naam van het domein moet worden opgegeven met de parameter 'Target'
* Als de methode 'Prestaties' Routering van verkeer wordt gebruikt, is de 'EndpointLocation' is vereist. Dit is anders is optioneel. De waarde moet een [geldig Azure-regionaam](https://azure.microsoft.com/regions/).
* De 'Gewicht' en 'Prioriteit' zijn optioneel.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Toe te voegen externe eindpunten met behulp van `Add-AzureRmTrafficManagerEndpointConfig` en `Set-AzureRmTrafficManagerProfile`

In dit voorbeeld wordt een Traffic Manager-profiel maken, twee externe eindpunten toevoegen, en de wijzigingen.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Externe eindpunten met behulp van toevoegen `New-AzureRmTrafficManagerEndpoint`

In dit voorbeeld wordt een externe eindpunt toevoegen aan een bestaand profiel. Het profiel is opgegeven met behulp van de namen van de groep profiel en de resource.

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>'Nested' eindpunten toevoegen

Elke Traffic Manager-profiel Hiermee geeft u een methode voor eenmalige routering van verkeer. Er zijn echter scenario's waarin meer geavanceerde routering van verkeer dan de routering geleverd door een enkele Traffic Manager-profiel. U kunt Traffic Manager-profielen als u wilt combineren van de voordelen van meer dan één methode voor verkeersroutering nesten. Geneste profielen kunnen u het standaardgedrag voor Traffic Manager voor ondersteuning voor grotere en complexere implementaties van toepassingen vervangen. Zie voor meer voorbeelden van gedetailleerde [geneste Traffic Manager-profielen](traffic-manager-nested-profiles.md).

Geneste eindpunten zijn geconfigureerd op het bovenliggende-profiel met behulp van een bepaald eindpunt-type 'NestedEndpoints'. Bij het opgeven van geneste eindpunten:

* Het eindpunt moet worden opgegeven met de parameter 'targetResourceId'
* Als de methode 'Prestaties' Routering van verkeer wordt gebruikt, is de 'EndpointLocation' is vereist. Dit is anders is optioneel. De waarde moet een [geldig Azure-regionaam](http://azure.microsoft.com/regions/).
* De 'Gewicht' en 'Prioriteit' zijn optioneel, als voor Azure-eindpunten.
* De parameter 'MinChildEndpoints' is optioneel. De standaardwaarde is '1'. Als het aantal beschikbare eindpunten onder deze drempelwaarde valt, wordt het profiel van de bovenliggende beschouwt het onderliggende profiel 'verminderde' en verkeer naar de andere eindpunten in het profiel van de bovenliggende zorgt.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Toe te voegen geneste eindpunten met behulp van `Add-AzureRmTrafficManagerEndpointConfig` en `Set-AzureRmTrafficManagerProfile`

In dit voorbeeld we nieuwe Traffic Manager onderliggende en bovenliggende profielen maken, het onderliggende object als een geneste eindpunt toevoegen aan de bovenliggende en de wijzigingen aan.

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Beknopt alternatief in dit voorbeeld hebt we niet alle andere eindpunten toevoegen aan de onderliggende of bovenliggende profielen.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Geneste eindpunten met behulp van toevoegen `New-AzureRmTrafficManagerEndpoint`

In dit voorbeeld toevoegen we een bestaand profiel van de onderliggende als een geneste eindpunt aan een bestaand profiel van de bovenliggende. Het profiel is opgegeven met behulp van de namen van de groep profiel en de resource.

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="adding-endpoints-from-another-subscription"></a>Eindpunten toevoegen vanuit een ander abonnement

Traffic Manager kunt werken met eindpunten uit verschillende abonnementen. U moet overschakelen naar het abonnement met het eindpunt dat u wilt toevoegen als u wilt ophalen van de benodigde invoer op Traffic Manager. Vervolgens moet u overschakelen naar de abonnementen, met Traffic Manager-profiel en de encpoint aan toe te voegen. Het onderstaande voorbeeld laat zien hoe u dit doet met een openbaar IP-adres.

```powershell
Set-AzureRmContext -SubscriptionId $EndpointSubscription
$ip = Get-AzureRmPublicIpAddress -Name $IpAddresName -ResourceGroupName $EndpointRG

Set-AzureRmContext -SubscriptionId $trafficmanagerSubscription
New-AzureRmTrafficManagerEndpoint -Name $EndpointName -ProfileName $ProfileName -ResourceGroupName $TrafficManagerRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="update-a-traffic-manager-endpoint"></a>Een Traffic Manager-eindpunt bijwerken

Er zijn twee manieren om bij te werken van een bestaand Traffic Manager-eindpunt:

1. Het Traffic Manager profiel met behulp van `Get-AzureRmTrafficManagerProfile`, bijwerken van de eigenschappen van het eindpunt binnen het profiel en de wijzigingen aan met behulp van `Set-AzureRmTrafficManagerProfile`. Deze methode heeft het voordeel van de mogelijkheid om bij te werken, meer dan één eindpunt in één bewerking.
2. Het Traffic Manager eindpunt met behulp van `Get-AzureRmTrafficManagerEndpoint`, bijwerken van de eigenschappen van het eindpunt en de wijzigingen aan met behulp van `Set-AzureRmTrafficManagerEndpoint`. Deze methode is eenvoudiger, omdat dit vereist niet dat bij de matrix eindpunten in het profiel te indexeren.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Voorbeeld 1: Bijwerken van de eindpunten met behulp van `Get-AzureRmTrafficManagerProfile` en `Set-AzureRmTrafficManagerProfile`

In dit voorbeeld wijzigen we de prioriteit op twee eindpunten binnen een bestaand profiel.

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Voorbeeld 2: Bijwerken van een eindpunt met `Get-AzureRmTrafficManagerEndpoint` en `Set-AzureRmTrafficManagerEndpoint`

In dit voorbeeld wijzigen we het gewicht van één eindpunt in een bestaand profiel.

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Eindpunten en -profielen inschakelen en uitschakelen

Traffic Manager kunt afzonderlijke eindpunten zijn ingeschakeld en uitgeschakeld, in- en uitschakelen van de gehele profielen biedt.
Deze wijzigingen kunnen worden gemaakt door ophalen/bijwerken/instelling het eindpunt of een profiel van resources. Voor het stroomlijnen van deze veelvoorkomende bewerkingen, worden ze ook ondersteund via toegewezen cmdlets.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Voorbeeld 1: In- en uitschakelen van een Traffic Manager-profiel

Als u wilt een Traffic Manager-profiel inschakelen, gebruikt u `Enable-AzureRmTrafficManagerProfile`. Het profiel kan worden opgegeven met behulp van een profiel-object. Het object van het profiel kan worden doorgegeven via de pijplijn of met behulp van de '-TrafficManagerProfile' parameter. In dit voorbeeld geven we het profiel met de naam van de profiel- en resource.

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Een Traffic Manager-profiel uitschakelen:

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

De cmdlet Disable-AzureRmTrafficManagerProfile vraagt om bevestiging. Deze vraag kan worden onderdrukt met behulp van de '-Force-parameter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Voorbeeld 2: In- en uitschakelen van een Traffic Manager-eindpunt

Als u wilt een Traffic Manager-eindpunt inschakelen, gebruikt u `Enable-AzureRmTrafficManagerEndpoint`. Er zijn twee manieren om op te geven van het eindpunt

1. Met behulp van een TrafficManagerEndpoint-object doorgegeven via de pijplijn of met behulp van de '-TrafficManagerEndpoint' parameter
2. Met behulp van de naam van het eindpunt, eindpunt van het type, profielnaam en de naam van resourcegroep:

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Op deze manier om uit te schakelen van een Traffic Manager-eindpunt:

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Net als bij `Disable-AzureRmTrafficManagerProfile`, wordt de `Disable-AzureRmTrafficManagerEndpoint` cmdlet vraagt om bevestiging. Deze vraag kan worden onderdrukt met behulp van de '-Force-parameter.

## <a name="delete-a-traffic-manager-endpoint"></a>Een Traffic Manager-eindpunt verwijderen

U kunt afzonderlijke eindpunten verwijderen met de `Remove-AzureRmTrafficManagerEndpoint` cmdlet:

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Deze cmdlet vraagt om bevestiging. Deze vraag kan worden onderdrukt met behulp van de '-Force-parameter.

## <a name="delete-a-traffic-manager-profile"></a>Een Traffic Manager-profiel verwijderen

Als u wilt een Traffic Manager-profiel verwijdert, gebruikt u de `Remove-AzureRmTrafficManagerProfile` cmdlet, namen van het profiel en de resource op te geven:

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Deze cmdlet vraagt om bevestiging. Deze vraag kan worden onderdrukt met behulp van de '-Force-parameter.

Het profiel moet worden verwijderd kan ook worden opgegeven met behulp van een profiel-object:

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
