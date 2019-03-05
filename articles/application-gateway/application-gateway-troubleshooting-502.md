---
title: Azure Application Gateway ongeldige Gateway (502) oplossen | Microsoft Docs
description: Meer informatie over het oplossen van fouten in Application Gateway 502
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: d50f25fbe10fc5ac4e834141fe7ac45fbed918ab
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57309023"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Het oplossen van fouten over ongeldige gateways in Application Gateway

Informatie over het oplossen van fouten over ongeldige gateways (502) ontvangen bij het gebruik van de toepassingsgateway.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Overzicht

Na het configureren van een application gateway is een van de fouten die gebruikers kunnen ondervinden ' Serverfout: 'Serverfout: 502 - De webserver heeft een ongeldig antwoord ontvangen terwijl deze fungeerde als gateway of proxyserver.' Deze fout kan optreden vanwege de volgende belangrijke redenen:

* NSG, UDR en aangepaste DNS wordt geblokkeerd door de toegang tot de back-endpoolleden.
* Back-end-VM's of exemplaren van virtuele-machineschaalset reageren niet op de standaard-statustest.
* Ongeldige of onjuiste configuratie van aangepaste statustests.
* Azure Application-Gateway [back-end-pool is niet geconfigureerd of lege](#empty-backendaddresspool).
* Geen van de virtuele machines of -exemplaren in [virtuele-machineschaalset in orde zijn](#unhealthy-instances-in-backendaddresspool).
* [Problemen met time-outperiode of connectiviteit aanvragen](#request-time-out) met aanvragen van gebruikers.

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Network Security Group, User Defined Route of aangepaste DNS-probleem

### <a name="cause"></a>Oorzaak

Als de toegang tot de back-end is geblokkeerd vanwege de aanwezigheid van NSG, UDR en aangepaste DNS-, wordt Application Gateway-instanties niet kunnen bereiken van de back-endpool en zou leiden tot testfouten 502 fouten veroorzaken. Houd er rekening mee dat de NSG/UDR kan aanwezig zijn in Application Gateway-subnet of het subnet waarin de toepassings-VM's zijn geïmplementeerd. Aanwezigheid van aangepaste DNS-server in het VNET kan op dezelfde manier ook problemen veroorzaken als de FQDN-naam wordt gebruikt voor back-endpoolleden en is niet opgelost correct door de gebruiker geconfigureerde DNS-server voor het VNET.

### <a name="solution"></a>Oplossing

NSG, UDR en DNS-configuratie valideren door te gaan via de volgende stappen uit:
* Controleer nsg's die zijn gekoppeld aan Application Gateway-subnet. Zorg ervoor dat de communicatie met de back-end niet is geblokkeerd.
* Controleer de UDR die zijn gekoppeld aan Application Gateway-subnet. Zorg ervoor dat UDR verkeer van back endsubnet niet leidt - bijvoorbeeld controleren voor de routering van virtuele apparaten of standaardroutes worden geadverteerd voor Application Gateway-subnet via ExpressRoute/VPN-netwerk.

```powershell
$vnet = Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Effectieve NSG en route met de back-end van de virtuele machine controleren

```powershell
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
Als aanwezig is, controleert u of de DNS-server correct kan omzetten van back-end-poollid FQDN-naam.

## <a name="problems-with-default-health-probe"></a>Problemen met de statustest standaard

### <a name="cause"></a>Oorzaak

502 fouten kunnen ook worden vaak indicatoren de standaard status-test is niet bereikbaar back-end VM's. Wanneer een toepassingsgateway-exemplaar is ingericht, configureert het automatisch een standaard-statustest aan elke BackendAddressPool met behulp van eigenschappen van de BackendHttpSetting. Er is geen gebruikersinvoer is vereist voor het instellen van deze test. Met name wanneer een regel voor taakverdeling is geconfigureerd, wordt een koppeling gemaakt tussen een BackendHttpSetting en BackendAddressPool. Een standaard-test is geconfigureerd voor elk van deze koppelingen en Application Gateway initieert een periodieke statuscontroles Controleer de verbinding voor elke instantie die in de BackendAddressPool op de poort is opgegeven in het element BackendHttpSetting. Volgende tabel bevat de waarden die zijn gekoppeld aan de standaard-statustest.

| Test-eigenschap | Value | Description |
| --- | --- | --- |
| Test-URL |http://127.0.0.1/ |URL-pad |
| Interval |30 |Testinterval in seconden |
| Time-out |30 |Test time-out in seconden |
| Drempelwaarde voor onjuiste status |3 |Aantal nieuwe pogingen-test. De back-endserver is gemarkeerd omlaag nadat het aantal opeenvolgende fouten de drempelwaarde voor onjuiste status heeft bereikt. |

### <a name="solution"></a>Oplossing

* Zorg ervoor dat een standaard-site is geconfigureerd en op 127.0.0.1 luistert.
* Als BackendHttpSetting Hiermee geeft u een andere poort dan 80, moet u de standaard-site geconfigureerd om te luisteren op poort.
* De aanroep van http://127.0.0.1:port moet de resultaatcode van een HTTP 200 retourneren. Dit moet worden geretourneerd binnen de time-outperiode van 30 seconden.
* Zorg ervoor dat poort geconfigureerd geopend is en dat er zijn geen firewallregels of Azure-Netwerkbeveiligingsgroepen, dit blokkeren van binnenkomend of uitgaand verkeer op de poort die is geconfigureerd.
* Als Azure klassieke virtuele machines of Cloud Service met de FQDN-naam of openbaar IP-adres wordt gebruikt, zorg ervoor dat de bijbehorende [eindpunt](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) wordt geopend.
* Als de virtuele machine is geconfigureerd via Azure Resource Manager en buiten de VNet waar Application Gateway is geïmplementeerd, [Network Security Group](../virtual-network/security-overview.md) moet worden geconfigureerd voor toegang op de gewenste poort.

## <a name="problems-with-custom-health-probe"></a>Problemen met aangepaste statustest

### <a name="cause"></a>Oorzaak

Aangepaste statustests kunnen extra flexibiliteit om de standaard probing gedrag. Wanneer u aangepaste tests, kunnen gebruikers het testinterval, de URL en pad om te testen en het aantal mislukte antwoorden om te accepteren voordat u markeert de instantie van de back-end-pool als niet in orde configureren. De volgende aanvullende eigenschappen zijn toegevoegd.

| Test-eigenschap | Description |
| --- | --- |
| Name |De naam van de test. Deze naam wordt gebruikt om te verwijzen naar de test in de back-end-HTTP-instellingen. |
| Protocol |Het protocol dat wordt gebruikt voor het verzenden van de test. De test wordt gebruikt voor het protocol dat is gedefinieerd in de back-end-HTTP-instellingen |
| Host |De naam van de host voor het verzenden van de test. Alleen van toepassing wanneer meerdere sites op Application Gateway is geconfigureerd. Dit wijkt af van de VM-hostnaam. |
| Pad |Relatief pad van de test. Ongeldig pad begint met '/'. De test wordt verzonden naar \<protocol\>://\<host\>:\<poort\>\<pad\> |
| Interval |Testinterval in seconden. Dit is het tijdsinterval tussen twee opeenvolgende tests. |
| Time-out |Test time-out in seconden. Als een geldige reactie niet binnen deze time-outperiode ontvangen is is, wordt de test is gemarkeerd als mislukt. |
| Drempelwaarde voor onjuiste status |Aantal nieuwe pogingen-test. De back-endserver is gemarkeerd omlaag nadat het aantal opeenvolgende fouten de drempelwaarde voor onjuiste status heeft bereikt. |

### <a name="solution"></a>Oplossing

Controleer of de Statustest voor de aangepaste correct als de voorgaande tabel is geconfigureerd. Naast de voorgaande stappen voor probleemoplossing, zorg er ook voor het volgende:

* Zorg ervoor dat de test correct is opgegeven als per de [handleiding](application-gateway-create-probe-ps.md).
* Als Application Gateway is geconfigureerd voor een enkele site, wordt standaard de Host moet naam worden opgegeven als '127.0.0.1', tenzij anders is geconfigureerd in aangepaste test.
* Zorg ervoor dat een aanroep naar http://\<host\>:\<poort\>\<pad\> retourneert de resultaatcode van een HTTP 200.
* Zorg ervoor dat Interval, Time-out en UnhealtyThreshold binnen een acceptabel prestatiebereik.
* Als u met behulp van een HTTPS-test, zorg ervoor dat de back-endserver zijn vereist om SNI door het configureren van een fallback-certificaat op de back endserver zelf.

## <a name="request-time-out"></a>Time-out voor aanvraag

### <a name="cause"></a>Oorzaak

Wanneer een aanvraag wordt ontvangen, kan Application Gateway de geconfigureerde regels toegepast op de aanvraag en doorgestuurd naar een exemplaar van de back-end-pool. Deze wacht gedurende een configureerbare tijdsinterval voor een reactie van de back-end-exemplaar. Dit interval is standaard **30 seconden**. Als Application Gateway geen een reactie van de back-endtoepassing in dit interval ontvangen biedt, ziet u gebruikersaanvraag een 502-fout.

### <a name="solution"></a>Oplossing

Application Gateway kan gebruikers deze instelling via BackendHttpSetting, die vervolgens kan worden toegepast op verschillende groepen wilt configureren. Verschillende back-end-pools kunnen verschillende BackendHttpSetting en kan daarom verschillende aanvraagtime geconfigureerd hebben.

```powershell
    New-AzApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Lege BackendAddressPool

### <a name="cause"></a>Oorzaak

Als de Application Gateway heeft geen virtuele machines of virtuele-machineschaalset geconfigureerd in de back-end-adresgroep, wordt elke klantaanvraag niet omleiden en genereert een fout met ongeldige gateway.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de back-end-adresgroep is niet leeg zijn. Dit kan worden gedaan via PowerShell, CLI of -portal.

```powershell
Get-AzApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

De uitvoer van de vorige cmdlet moet niet-lege back-end-adresgroep bevatten. Hieronder volgt een voorbeeld waarin twee groepen worden geretourneerd die zijn geconfigureerd met de FQDN-naam of IP-adressen voor back-end VM's. De Inrichtingsstatus van de BackendAddressPool moet 'geslaagd'.

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

Als alle instanties van BackendAddressPool niet in orde zijn, klikt u vervolgens hoeft Application Gateway niet back-end-to-route gebruikersaanvraag tot. Dit kan ook het geval zijn wanneer back-end-exemplaren in orde zijn, maar niet de vereiste toepassing geïmplementeerd hoeft.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de exemplaren in orde zijn en de toepassing correct is geconfigureerd. Controleer of de back-end-exemplaren kunnen reageren op een ping naar een andere virtuele machine in hetzelfde VNet. Als met een openbaar eindpunt is geconfigureerd, controleert u of de aanvraag van een browser naar de webtoepassing onderhouden.

## <a name="next-steps"></a>Volgende stappen

Als de voorgaande stappen het probleem niet verhelpen, opent u een [ondersteuningsticket](https://azure.microsoft.com/support/options/).

