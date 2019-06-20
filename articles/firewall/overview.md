---
title: Wat is Azure Firewall?
description: Meer informatie over Azure Firewall-functies.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 6/20/2019
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: ace0b56ce1ba4c140666c8f2dd6e2187f479446e
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272642"
---
# <a name="what-is-azure-firewall"></a>Wat is Azure Firewall?

Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Er is een volledig stateful firewall als een service met ingebouwde hoge beschikbaarheid en cloudschaalbaarheid van de onbeperkte.

![Firewalloverzicht](media/overview/firewall-threat.png)

U kunt beleid voor toepassings- en netwerkconnectiviteit centraal maken, afdwingen en registreren voor abonnementen en virtuele netwerken. Azure Firewall maakt gebruik van een statisch openbaar IP-adres voor uw virtuele-netwerkresources zodat externe firewalls verkeer dat afkomstig is van uw virtuele netwerk kunnen identificeren.  De service is volledig geïntegreerd met Azure Monitor voor registratie en analyses.

## <a name="features"></a>Functies

Azure Firewall biedt de volgende functies:

### <a name="built-in-high-availability"></a>Ingebouwde hoge beschikbaarheid

Hoge beschikbaarheid is ingebouwd in, zodat er zijn geen extra load balancers vereist zijn en er u hoeft niets is te configureren.

### <a name="availability-zones-public-preview"></a>Beschikbaarheidszones (preview-versie)

Firewall van Azure kan worden geconfigureerd tijdens de implementatie in meerdere Beschikbaarheidszones voor verhoogde beschikbaarheid omvatten. Met Beschikbaarheidszones, wordt de beschikbaarheid van uw verhoogd naar bedrijfstijd van 99,99%. Zie voor meer informatie, de Azure-Firewall [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). De bedrijfstijd van 99,99% SLA wordt aangeboden wanneer twee of meer Beschikbaarheidszones zijn geselecteerd.

U kunt Azure-Firewall ook koppelen aan een specifieke zone alleen omwille van de nabijheid, met behulp van de service standaard 99,95% SLA.

Er is geen extra kosten voor een firewall in een Beschikbaarheidszone is geïmplementeerd. Er zijn echter extra kosten voor binnenkomende en uitgaande gegevensoverdracht die zijn gekoppeld met Beschikbaarheidszones. Zie voor meer informatie, [prijsinformatie voor bandbreedte](https://azure.microsoft.com/pricing/details/bandwidth/).

Azure-Beschikbaarheidszones Firewall zijn beschikbaar in regio's die ondersteuning voor Beschikbaarheidszones. Zie voor meer informatie, [wat zijn Beschikbaarheidszones in Azure?](../availability-zones/az-overview.md#services-support-by-region)

> [!NOTE]
> Beschikbaarheidszones kan alleen worden geconfigureerd tijdens de implementatie. U kunt een bestaande firewall om op te nemen van de Beschikbaarheidszones niet configureren.

Zie voor meer informatie over Beschikbaarheidszones [wat zijn Beschikbaarheidszones in Azure?](../availability-zones/az-overview.md)

### <a name="unrestricted-cloud-scalability"></a>Onbeperkte cloudschaalbaarheid

U kunt Azure Firewall omhoog schalen zoveel als nodig is om te voldoen aan veranderende netwerkverkeersstromen, zodat u geen budget hoeft te reserveren voor het drukste verkeer.

### <a name="application-fqdn-filtering-rules"></a>Regels voor het filteren van de FQDN van toepassingen

U kunt uitgaand HTTP/S-verkeer beperken tot een opgegeven lijst met volledig gekwalificeerde domeinnamen (FQDN), inclusief jokertekens. Deze functie is geen SSL-beëindiging vereist.

### <a name="network-traffic-filtering-rules"></a>Regels voor het filteren van netwerkverkeer

U kunt de netwerkfilterregels *toestaan* of *weigeren* centraal maken op IP-adres van bron en doel, poort en protocol. Azure Firewall is volledig stateful, wat betekent dat het legitieme pakketten voor verschillende soorten verbindingen kan onderscheiden. Regels worden afgedwongen en vastgelegd voor meerdere abonnementen en virtuele netwerken.

### <a name="fqdn-tags"></a>FQDN-tags

Met FQDN-tags kunt u eenvoudig bekend netwerkverkeer voor Azure-services toestaan in uw firewall. Stel dat u Windows Update-netwerkverkeer wilt toestaan in de firewall. U maakt een toepassingsregel die de Windows Update-tag bevat. Het netwerkverkeer van Windows Update kan nu door uw firewall.

### <a name="service-tags"></a>Servicetags

Een servicetag vertegenwoordigt een groep IP-adresvoorvoegsels die het maken van beveiligingsregel vereenvoudigt. U kan niet uw eigen servicetag maken en ook niet opgeven welke IP-adressen zijn opgenomen in een tag. Microsoft beheert de adresvoorvoegsels die de servicetag omvat en werkt de servicetag automatisch bij wanneer adressen veranderen.

### <a name="threat-intelligence"></a>Informatie over bedreigingen

Filteren op basis van bedreigingsinformatie kan voor uw firewall worden ingeschakeld voor waarschuwingen over of het weigeren van verkeer van en naar bekende kwaadaardige IP-adressen en domeinen. De IP-adressen en domeinen zijn afkomstig uit de feed Bedreigingsinformatie van Microsoft.

### <a name="outbound-snat-support"></a>Ondersteuning voor uitgaande SNAT

Alle uitgaande IP-adressen van virtueel netwerkverkeer worden geconverteerd naar de openbare IP van Azure Firewall (Source Network Address Translation). U kunt verkeer dat afkomstig is uit uw virtuele netwerk naar externe internetbestemmingen identificeren en toestaan.

### <a name="inbound-dnat-support"></a>Ondersteuning voor inkomende DNAT

Het inkomende netwerkverkeer op het openbare IP-adres van de firewall wordt omgezet (Destination Network Address Translation) en gefilterd op het privé-IP-adres in uw virtuele netwerken.

### <a name="multiple-public-ips-public-preview"></a>Meerdere openbare IP-adressen (openbare preview)

U kunt meerdere openbare IP-adressen (maximaal 600) koppelen aan uw firewall.

Hiermee kunt de volgende scenario's:

- **DNAT** -u kunt meerdere exemplaren van de standaardpoort vertalen naar uw back-endservers. Als u twee openbare IP-adressen hebt, kunt u bijvoorbeeld TCP-poort 3389 (RDP) voor beide IP-adressen vertalen.
- **SNAT** -aanvullende poorten zijn beschikbaar voor uitgaande SNAT-verbindingen, verminderen de kans op poortuitputting SNAT. Op dit moment selecteert Azure Firewall willekeurig het openbare IP-bronadres te gebruiken voor een verbinding. Als u een downstream filteren op uw netwerk hebt, moet u alle openbare IP-adressen die zijn gekoppeld aan uw firewall toestaan.

> [!NOTE]
> Tijdens de preview-versie, als u toevoegen of verwijderen van een openbaar IP-adres aan een actieve firewall bestaande binnenkomende verbindingen met behulp van regels DNAT werkt mogelijk niet voor 40-120 seconden.

### <a name="azure-monitor-logging"></a>Logboekregistratie van Azure Monitor

Alle gebeurtenissen zijn geïntegreerd met Azure Monitor, zodat u logboeken kunt archiveren in een opslagaccount, gebeurtenissen kunt streamen naar uw Event Hub of deze kunt verzenden naar Azure Monitor-logboeken.

## <a name="known-issues"></a>Bekende problemen

Azure Firewall heeft de volgende bekende problemen:

|Probleem  |Description  |Oplossing  |
|---------|---------|---------|
|Conflict met de JIT-functie (Just-in-Time) van Azure Security Center (ASC)|Als een virtuele machine wordt geopend met behulp van JIT en zich in een subnet bevindt met een door de gebruiker gedefinieerde route die naar Azure Firewall als een standaardgateway verwijst, werkt ASC JIT niet. Dit is het gevolg van asymmetrische routering: Er is een pakket beschikbaar in via het openbare IP van de virtuele machine (JIT geopend voor toegang tot de), maar de return pad is via de firewall, die het pakket wordt geweigerd omdat er geen vastgestelde sessie op de firewall.|Om dit probleem te omzeilen, plaatst u de virtuele JIT-machines op een apart subnet dat geen door de gebruiker gedefinieerde route naar de firewall bevat.|
Netwerkfilterregels voor niet-TCP/UDP-protocollen (bijvoorbeeld ICMP) werken niet voor internetverkeer|Netwerkfilterregels voor niet-TCP/UDP-protocollen werken niet met SNAT naar uw openbare IP-adres. Niet-TCP/UDP-protocollen worden ondersteund tussen spoke-subnetten en VNets.|Azure Firewall maakt gebruik van de standaardversie van Standard Load Balancer, [die momenteel geen ondersteuning biedt voor SNAT voor IP-protocollen](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations). We verkent opties voor de ondersteuning van dit scenario in een toekomstige release.|
|Ontbrekende PowerShell- en CLI-ondersteuning voor ICMP|Azure PowerShell en CLI bieden geen ondersteuning voor ICMP als een geldig protocol in netwerkregels.|Het is nog steeds mogelijk gebruik van ICMP als protocol via de portal en de REST-API. Er wordt geprobeerd om toe te voegen van de ICMP binnenkort in PowerShell en CLI.|
|FQDN-tags vereisen instelling van een protocol: poort|Toepassing met FQDN-naam tags-regelgeving eist poort: definitie-protocol.|U kunt **https** gebruiken als de waarde voor poort:protocol. Er wordt geprobeerd om te zorgen dat dit veld optioneel wanneer FQDN labels worden gebruikt.|
|Een firewall verplaatsen naar een andere resourcegroep of abonnement wordt niet ondersteund|Een firewall verplaatsen naar een andere resourcegroep of abonnement wordt niet ondersteund.|Ondersteuning van deze functionaliteit staat op de planning. Om een firewall naar een andere resourcegroep of ander abonnement verplaatsen, moet u het huidige exemplaar verwijderen en deze vervolgens opnieuw maken in de nieuwe resourcegroep of het nieuwe abonnement.|
|Poortbereik in netwerk- en toepassingsregels|Het aantal poorten is beperkt tot 64.000 omdat hoge poorten zijn gereserveerd voor beheer en statustests. |Er wordt geprobeerd om deze beperking te versoepelen.|
|Waarschuwingen van Threat intelligence mogelijk ophalen gemaskeerd.|Netwerkregels met doel 80/443 voor uitgaande filteren maskers threat intelligence waarschuwingen wanneer geconfigureerd voor de waarschuwing alleen-modus.|Maak met behulp van regels voor het toepassen van 80/443 uitgaande filteren. Of wijzig de modus threat intelligence voor **waarschuwen en weigeren**.|
|Firewall van Azure maakt gebruik van Azure DNS alleen voor het omzetten van|Firewall van Azure wordt omgezet FQDN-namen alleen met behulp van Azure DNS. Een aangepaste DNS-server wordt niet ondersteund. Er is geen invloed op DNS-omzetting in andere subnetten.|Er wordt geprobeerd om deze beperking te versoepelen.|
|Azure Firewall SNAT/DNAT werkt niet voor persoonlijke IP-bestemmingen|Ondersteuning van Azure Firewall SNAT/DNAT is beperkt tot Internet uitgaand/inkomende. SNAT/DNAT werkt momenteel niet voor persoonlijke IP-doelen. Bijvoorbeeld, spoke-knooppunt.|Dit is een beperking.|
|Eerste openbare IP-adres verwijderen niet|U kunt de eerste openbare IP-adres toegewezen aan de firewall, tenzij de firewall is ongedaan gemaakt of verwijderd niet verwijderen.|Dit is standaard.|
|Als u toevoegen of verwijderen van een openbaar IP-adres, werken de DNAT regels mogen niet tijdelijk.| Als u toevoegen of verwijderen van een openbaar IP-adres aan een actieve firewall, werkt mogelijk niet bestaande binnenkomende verbindingen met behulp van regels DNAT 40-120 seconden.|Dit is een beperking van de openbare preview-versie voor deze functie.|
|Beschikbaarheidszones kunnen alleen worden geconfigureerd tijdens de implementatie.|Beschikbaarheidszones kunnen alleen worden geconfigureerd tijdens de implementatie. U kunt de Beschikbaarheidszones niet configureren nadat een firewall is geïmplementeerd.|Dit is standaard.|

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Azure Firewall implementeren en configureren met Azure Portal](tutorial-firewall-deploy-portal.md)
- [Azure Firewall implementeren met behulp van een sjabloon](deploy-template.md)
- [Een Azure Firewall-testomgeving maken](scripts/sample-create-firewall-test.md)
