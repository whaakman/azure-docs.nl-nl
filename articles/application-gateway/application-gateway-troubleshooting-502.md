---
title: Azure Application Gateway ongeldige Gateway (502) oplossen
description: Meer informatie over het oplossen van fouten in Application Gateway 502
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: amsriva
ms.openlocfilehash: 2a1c7e480e896da6852949c9d765d17290e4e9ce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64697168"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Het oplossen van fouten over ongeldige gateways in Application Gateway

Informatie over het oplossen van fouten over ongeldige gateways (502) ontvangen bij het gebruik van Azure Application Gateway.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht

Na het configureren van een application gateway, een van de fouten die mogelijk worden weergegeven is ' Serverfout: 'Serverfout: 502 - De webserver heeft een ongeldig antwoord ontvangen terwijl deze fungeerde als gateway of proxyserver.' Deze fout kan optreden voor de volgende belangrijke redenen:

* NSG, UDR en aangepaste DNS wordt geblokkeerd door de toegang tot de back-endpoolleden.
* Back-end-VM's of exemplaren van virtuele-machineschaalset zijn niet reageert op de standaard-statustest.
* Ongeldige of onjuiste configuratie van aangepaste statustests.
* Azure Application-Gateway [back-end-pool is niet geconfigureerd of lege](#empty-backendaddresspool).
* Geen van de virtuele machines of -exemplaren in [virtuele-machineschaalset in orde zijn](#unhealthy-instances-in-backendaddresspool).
* [Problemen met time-outperiode of connectiviteit aanvragen](#request-time-out) met aanvragen van gebruikers.

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Network Security Group, User Defined Route of aangepaste DNS-probleem

### <a name="cause"></a>Oorzaak

Als de toegang tot de back-end is geblokkeerd vanwege een NSG, UDR en aangepaste DNS, kunnen de back-endpool application gateway-instanties niet bereiken. Dit zorgt ervoor dat de test mislukt, wat resulteert in 502 fouten.

De NSG/UDR kan worden gebruikt in het subnet van de gateway of het subnet waarin de toepassings-VM's zijn geïmplementeerd.

Op deze manier kan de aanwezigheid van een aangepaste DNS-server in het VNet ook problemen veroorzaken. Een FQDN-naam gebruikt voor back-endpoolleden mogelijk niet correct opgelost door de gebruiker geconfigureerde DNS-server voor het VNet.

### <a name="solution"></a>Oplossing

NSG, UDR en DNS-configuratie valideren door te gaan via de volgende stappen uit:

* Controleer nsg's die zijn gekoppeld aan het subnet van de gateway. Zorg ervoor dat de communicatie met de back-end wordt niet geblokkeerd.
* Controleer de UDR die zijn gekoppeld aan het subnet van de gateway. Zorg ervoor dat de UDR wordt niet worden gebruikt voor het routeren van verkeer van het back endsubnet. Controleer bijvoorbeeld voor de routering van virtuele apparaten of standaardroutes worden geadverteerd voor het subnet van de gateway via ExpressRoute/VPN-netwerk.

```azurepowershell
$vnet = Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Effectieve NSG en route met de back-end van de virtuele machine controleren

```azurepowershell
Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName nic1 -ResourceGroupName testrg
Get-AzEffectiveRouteTable -NetworkInterfaceName nic1 -ResourceGroupName testrg
```

* Controleer de aanwezigheid van aangepaste DNS-server in het VNet. DNS kan worden gecontroleerd door te kijken details van de VNet-eigenschappen in de uitvoer.

```json
Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName 
DhcpOptions            : {
                           "DnsServers": [
                             "x.x.x.x"
                           ]
                         }
```
Als dat aanwezig is, zorgt u ervoor dat de DNS-server de back-end-poollid Fully Qualified Domain Name correct kan omzetten.

## <a name="problems-with-default-health-probe"></a>Problemen met de statustest standaard

### <a name="cause"></a>Oorzaak

502 fouten kunnen ook worden vaak indicatoren dat de standaard-statustest back-end VM's niet kan bereiken.

Wanneer een toepassingsgateway-exemplaar is ingericht, configureert het automatisch een standaard-statustest aan elke BackendAddressPool met behulp van eigenschappen van de BackendHttpSetting. Er is geen gebruikersinvoer is vereist voor het instellen van deze test. Met name wanneer een regel voor taakverdeling is geconfigureerd, wordt een koppeling gemaakt tussen een BackendHttpSetting en een BackendAddressPool. Een standaard-test is geconfigureerd voor elk van deze koppelingen en de application gateway start een periodieke statuscontroles Controleer de verbinding voor elke instantie die in de BackendAddressPool op de poort is opgegeven in het element BackendHttpSetting. 

De volgende tabel bevat de waarden die zijn gekoppeld aan de standaard-statustest:

| Test-eigenschap | Value | Description |
| --- | --- | --- |
| WebTest-URL |`http://127.0.0.1/` |URL-pad |
| Interval |30 |Testinterval in seconden |
| Time-out |30 |Test time-out in seconden |
| Drempelwaarde voor onjuiste status |3 |Aantal nieuwe pogingen-test. De back-endserver is gemarkeerd omlaag nadat het aantal opeenvolgende fouten de drempelwaarde voor onjuiste status heeft bereikt. |

### <a name="solution"></a>Oplossing

* Zorg ervoor dat een standaard-site is geconfigureerd en op 127.0.0.1 luistert.
* Als BackendHttpSetting Hiermee geeft u een andere poort dan 80, moet u de standaard-site geconfigureerd om te luisteren op poort.
* De aanroep van `http://127.0.0.1:port` moet de resultaatcode van een HTTP 200 retourneren. Dit moet worden geretourneerd in de 30 seconden time-outperiode.
* Zorg ervoor dat de poort die is geconfigureerd geopend is en dat er zijn geen firewallregels of Azure-Netwerkbeveiligingsgroepen, dit blokkeren van binnenkomend of uitgaand verkeer op de poort die is geconfigureerd.
* Als Azure klassieke virtuele machines of Cloud Service met een FQDN-naam of een openbaar IP-adres wordt gebruikt, zorg ervoor dat de bijbehorende [eindpunt](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) wordt geopend.
* Als de virtuele machine is geconfigureerd via Azure Resource Manager en buiten de VNet waar de application gateway is geïmplementeerd, een [Network Security Group](../virtual-network/security-overview.md) moet worden geconfigureerd voor toegang op de gewenste poort.

## <a name="problems-with-custom-health-probe"></a>Problemen met aangepaste statustest

### <a name="cause"></a>Oorzaak

Aangepaste statustests kunnen extra flexibiliteit om de standaard probing gedrag. Wanneer u aangepaste tests, kunt u het testinterval, de URL, het pad voor het testen van en het aantal mislukte antwoorden om te accepteren voordat u markeert de instantie van de back-end-pool als niet in orde.

De volgende aanvullende eigenschappen zijn toegevoegd:

| Test-eigenschap | Description |
| --- | --- |
| Name |De naam van de test. Deze naam wordt gebruikt om te verwijzen naar de test in de back-end-HTTP-instellingen. |
| Protocol |Het protocol dat wordt gebruikt voor het verzenden van de test. De test wordt gebruikt voor het protocol dat is gedefinieerd in de back-end-HTTP-instellingen |
| Host |De naam van de host voor het verzenden van de test. Alleen van toepassing wanneer meerdere locaties is geconfigureerd op de application gateway. Dit wijkt af van de VM-hostnaam. |
| Pad |Relatief pad van de test. Ongeldig pad begint met '/'. De test wordt verzonden naar \<protocol\>://\<host\>:\<poort\>\<pad\> |
| Interval |Testinterval in seconden. Dit is het tijdsinterval tussen twee opeenvolgende tests. |
| Time-out |Test time-out in seconden. Als een geldige reactie is niet binnen deze time-outperiode ontvangen is, wordt de test is gemarkeerd als mislukt. |
| Drempelwaarde voor onjuiste status |Aantal nieuwe pogingen-test. De back-endserver is gemarkeerd omlaag nadat het aantal opeenvolgende fouten de drempelwaarde voor onjuiste status heeft bereikt. |

### <a name="solution"></a>Oplossing

Controleer of de Statustest voor de aangepaste correct als de voorgaande tabel is geconfigureerd. Naast de voorgaande stappen voor probleemoplossing, zorg er ook voor het volgende:

* Zorg ervoor dat de test correct is opgegeven als per de [handleiding](application-gateway-create-probe-ps.md).
* Als de application gateway is geconfigureerd voor een enkele site, wordt standaard de Host naam moet worden opgegeven als `127.0.0.1`, tenzij anders is geconfigureerd in aangepaste test.
* Zorg ervoor dat een aanroep naar http://\<host\>:\<poort\>\<pad\> retourneert de resultaatcode van een HTTP 200.
* Zorg ervoor dat Interval, time-out en UnhealtyThreshold binnen een acceptabel prestatiebereik.
* Als u met behulp van een HTTPS-test, zorg ervoor dat de back-endserver zijn vereist om SNI door het configureren van een fallback-certificaat op de back endserver zelf.

## <a name="request-time-out"></a>Time-out voor aanvraag

### <a name="cause"></a>Oorzaak

Wanneer een aanvraag wordt ontvangen, wordt de toepassingsgateway de geconfigureerde regels toegepast op de aanvraag en stuurt deze naar een exemplaar van de back-end-pool. Deze wacht gedurende een configureerbare tijdsinterval voor een reactie van de back-end-exemplaar. Dit interval is standaard **20** seconden. Als de application gateway geen een reactie van de back-endtoepassing in dit interval ontvangen biedt, wordt de aanvraag van de gebruiker een 502-fout.

### <a name="solution"></a>Oplossing

Application Gateway kunt u deze instelling via de BackendHttpSetting, die vervolgens kan worden toegepast op verschillende groepen wilt configureren. Verschillende back-end-pools kunnen hebben verschillende BackendHttpSetting en een andere aanvraag time-out geconfigureerd.

```azurepowershell
    New-AzApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Lege BackendAddressPool

### <a name="cause"></a>Oorzaak

Als de application gateway heeft geen virtuele machines of virtuele-machineschaalset geconfigureerd in de back-end-adresgroep, wordt elke klantaanvraag niet omleiden en verzendt een ongeldige gateway-fout.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de back-end-adresgroep niet leeg zijn. Dit kan worden gedaan via PowerShell, CLI of -portal.

```azurepowershell
Get-AzApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

De uitvoer van de vorige cmdlet moet niet-lege back-end-adresgroep bevatten. Het volgende voorbeeld ziet twee groepen geretourneerd die zijn geconfigureerd met een FQDN-naam of een IP-adressen voor de back-end-VM's. De Inrichtingsstatus van de BackendAddressPool moet 'geslaagd'.

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>Exemplaren in BackendAddressPool

### <a name="cause"></a>Oorzaak

Als alle instanties van BackendAddressPool niet in orde zijn, klikt u vervolgens de application gateway beschikt niet over een back-end-to-route gebruikersaanvraag tot. Dit kan ook het geval zijn wanneer back-end-exemplaren in orde zijn, maar niet de vereiste toepassing geïmplementeerd hoeft.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de exemplaren in orde zijn en de toepassing correct is geconfigureerd. Controleer als de back-endexemplaren op een ping naar een andere virtuele machine in hetzelfde VNet reageren kunnen. Als met een openbaar eindpunt is geconfigureerd, controleert u of dat de aanvraag van een browser naar de webtoepassing wordt onderhouden.

## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet verhelpen, opent u een [ondersteuningsticket](https://azure.microsoft.com/support/options/).

