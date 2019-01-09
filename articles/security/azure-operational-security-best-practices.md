---
title: Azure best practices voor operationele beveiliging | Microsoft Docs
description: Dit artikel bevat een set van aanbevolen procedures voor operationele beveiliging van Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: terrylan
ms.openlocfilehash: 7c6d373fd294645605815d8a8d380259982d90e7
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118180"
---
# <a name="azure-operational-security-best-practices"></a>Azure best practices voor operationele beveiliging
Azure operationele beveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere assets in Azure. Operationele beveiliging in Azure is gebouwd op een framework waarin de kennis opgedaan uit mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl), wordt de [Microsoft Security Response Center](https://www.microsoft.com/msrc?rtc=1) programma en een diep besef van het landschap van cyberveiligheidsbedreigingen.

In dit artikel wordt besproken hoe een verzameling van aanbevolen procedures voor beveiliging. Deze aanbevolen procedures zijn afgeleid van onze ervaring met beveiliging van de Azure database en de ervaringen van klanten zoals zelf.

Voor elke best practice wordt we het volgende uitgelegd:
-   Wat de beste manier is
-   Waarom u deze wilt inschakelen, die best practices
-   Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
- Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

In dit artikel Azure operationele Best Practices voor beveiliging is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, zoals ze bestaan op het moment dat dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Storage-services controleren voor onverwachte wijzigingen in werking
Vaststellen en oplossen van problemen in een gedistribueerde toepassing die wordt gehost in een cloudomgeving kunnen zijn complexer dan in traditionele-omgevingen. Toepassingen kunnen worden geïmplementeerd in een PaaS of IaaS-infrastructuur, on-premises, op een mobiel apparaat, of in een combinatie van deze omgevingen. Netwerkverkeer van uw toepassing kan passeren openbare en particuliere netwerken en toepassingen kan gebruikmaken van meerdere opslagtechnologieën.

De storage-services die uw toepassing wordt gebruikt voor eventuele onverwachte wijzigingen in gedrag (zoals tragere responstijden), moet u continu controleren. Logboekregistratie gebruiken om meer gedetailleerde gegevens te verzamelen en analyseren van een probleem in de diepte. De diagnostische gegevens die u via controle en logboekregistratie aanschaft helpt u bij het bepalen van de hoofdoorzaak van het probleem dat uw toepassing is opgetreden. U kunt vervolgens het probleem oplossen met en bepaal de juiste stappen worden opgelost.

[Azure Storage Analytics](../storage/storage-analytics.md) logboekregistratie uitvoert en metrische gegevens biedt voor Azure storage-account. U wordt aangeraden dat u deze gegevens gebruikt om aanvragen te traceren, gebruikstrends te analyseren en problemen diagnosticeren met uw storage-account.

## <a name="prevent-detect-and-respond-to-threats"></a>Voorkomen, detecteren en direct reageren op bedreigingen
[Azure Security Center](../security-center/security-center-intro.md) helpt u bij het voorkomen, detecteren, en reageer op bedreigingen met verbeterde zichtbaarheid (en controle over) de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met verschillende beveiligingsoplossingen te detecteren.

De gratis laag van Security Center biedt beperkte beveiliging voor uw Azure-resources alleen. De prijscategorie Standard biedt deze mogelijkheden ook voor on-premises en andere clouds. Standard van Security Center helpt u beveiligingsproblemen te vinden en op te lossen, toegangs- en toepassingsbesturingselementen toe te passen om schadelijke activiteiten te blokkeren, bedreigingen te detecteren met behulp van analyses en gegevens en snel te reageren bij aanvallen. U kunt Security Center Standard de eerste 60 dagen kosteloos proberen. We raden aan dat u [onboarding uw Azure-abonnement naar Security Center Standard](../security-center/security-center-get-started.md).

Security Center gebruiken voor een centraal overzicht van de beveiligingsstatus van al uw Azure-resources. In een oogopslag controleren of de juiste beveiligingscontroles geïnstalleerd zijn en correct geconfigureerd en snel zien of er resources waarvoor aandacht.

## <a name="monitor-end-to-end-scenario-based-network-monitoring"></a>End-to-end scenario's gebaseerde netwerkbewaking bewaken
Klanten een end-to-end-netwerk in Azure bouwen door een combinatie van netwerkresources, zoals een virtueel netwerk, ExpressRoute, Application Gateway, en load balancers. Bewaking is beschikbaar op elk van de netwerkbronnen.

[Azure Network Watcher](../network-watcher/network-watcher-monitoring-overview.md) is een regionale service. Gebruik de hulpprogramma's voor diagnose en visualisatie te bewaken en diagnoses uitvoeren omstandigheden op netwerkscenarioniveau in, naar en van Azure.

De volgende zijn aanbevolen procedures voor het hulpprogramma's voor network bewakings- en beschikbaar.

**Beste**: Externe netwerkbewaking met packetopname automatiseren.  
**Details**: Bewaken en diagnoses uitvoeren netwerkproblemen zonder aan te melden bij uw virtuele machines met behulp van Network Watcher. Trigger [pakketopname](../network-watcher/network-watcher-alert-triggered-packet-capture.md) door waarschuwingen instellen en toegang krijgen tot informatie over realtime prestaties op packetniveau. Wanneer er een probleem wordt vastgesteld, kunt u dat uitgebreid onderzoeken voor een gedetailleerde diagnose.

**Beste**: Inzicht verkrijgen in uw netwerkverkeer met behulp van Logboeken van de stroom.  
**Details**: Een beter inzicht in uw netwerk verkeerspatronen bouwen met behulp van [stroomlogboeken van het netwerk](../network-watcher/network-watcher-nsg-flow-logging-overview.md). Informatie in stroomlogboeken kunt u gegevens verzamelen voor naleving, controleren en bewaken van het beveiligingsprofiel van uw netwerk.

**Beste**: VPN-connectiviteitsproblemen vaststellen.  
**Details**: Gebruik van Network Watcher te [vaststellen van de meest voorkomende problemen met VPN-Gateway en verbinding](../network-watcher/network-watcher-diagnose-on-premises-connectivity.md). U kunt niet alleen vaststellen van het probleem, maar ook gedetailleerde logboeken gebruiken voor het verder kunnen onderzoeken.

## <a name="secure-deployment-by-using-proven-devops-tools"></a>Implementatie beveiligen met behulp van bewezen DevOps-hulpprogramma 's
Gebruik de volgende aanbevolen procedures voor DevOps om ervoor te zorgen dat uw onderneming en teams productief en efficiënt zijn.

**Beste**: De build en implementatie van services automatiseren.  
**Details**: [Infrastructuur als code](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code) is een set van technieken en procedures waarmee IT-professionals het ongemak van het dagelijkse build en beheer van modulaire infrastructuur verwijderen. Hiermee kunnen IT-professionals om te bouwen en onderhouden van hun moderne serveromgeving op een manier die lijkt op hoe software-ontwikkelaars ontwikkelen en onderhouden van de toepassingscode.

U kunt [Azure Resource Manager](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) uw toepassingen inrichten met behulp van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U dezelfde sjabloon gebruiken om uw toepassing in elke fase van de levensduur van toepassingen herhaaldelijk te implementeren.

**Beste**: Automatisch bouwen en implementeren in Azure WebApps of cloudservices.  
**Details**: U kunt Azure die leiden tot [automatisch bouwen en implementeren van](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) op Azure-web-apps of cloudservices. Azure pijplijnen implementeert automatisch de binaire bestanden hierna een build naar Azure na elke check code in. Het bouwproces pakket komt overeen met de opdracht pakket in Visual Studio en de publicatie stappen zijn gelijk aan de opdracht Publish in Visual Studio.

**Beste**: Continue implementatie gebruiken.  
**Details**: [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/index?view=vsts) is een oplossing voor de implementatie van meerdere fasen automatiseren en beheren van de release-proces. Beheerde continue implementatie pijplijnen om snel, eenvoudig en vaak vrij te maken. Met Azure-pijplijnen, kunt u uw release-proces automatiseren en u kunt beschikken over vooraf gedefinieerde goedkeuringswerkstromen. Implementeer on-premises en naar de cloud uitbreiden en aanpassen zoals vereist.

**Beste**: Controleer de prestaties van uw app voordat u het starten of updates naar productie implementeren.  
**Details**: Uitvoeren van cloud-gebaseerde [belastingstests](https://docs.microsoft.com/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) met behulp van Azure-Test om te plannen:

- Prestatieproblemen in uw app vinden.
- Implementatie-kwaliteit verbeteren.
- Zorg ervoor dat uw app altijd beschikbaar is.
- Zorg ervoor dat uw app kan worden gebruikt voor het afhandelen van verkeer voor uw volgende starten of marketing-campagne.

**Beste**: Bewaak de prestaties van toepassingen.  
**Details**: [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) is een service extensible application performance management (APM) voor webontwikkelaars op meerdere platforms. Application Insights gebruiken om uw livewebtoepassing controleren. Er wordt automatisch gedetecteerd prestatieafwijkingen. Het bevat analytics-hulpprogramma's kunt u problemen identificeren en te begrijpen wat gebruikers daadwerkelijk doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.

## <a name="mitigate-and-protect-against-ddos"></a>Te beperken en te beveiligen tegen DDoS
Distributed denial of service (DDoS) is een soort aanval waarmee wordt geprobeerd te weinig toepassingsresources. Het doel is van invloed op de beschikbaarheid van de toepassing en de mogelijkheid voor het verwerken van legitieme aanvragen. Deze aanvallen worden steeds meer geavanceerde en grotere in omvang en impact. Ze kunnen worden gericht op een willekeurig eindpunt dat openbaar bereikbaar is via het internet.

Het ontwerpen en bouwen voor DDoS-bescherming vereist plannen en ontwerpen voor tal van foutmodi.

Hieronder vindt u aanbevolen procedures voor het bouwen van robuuste DDoS services op Azure.

**Beste**: Zorg ervoor dat beveiliging een prioriteit gedurende de hele levenscyclus van een toepassing, van ontwerp en implementatie tot implementatie en bewerkingen. Toepassingen kunnen fouten optreden die een relatief laag volume aan aanvragen voor het gebruik van een groot aantal bronnen, wat resulteert in een serviceonderbreking toestaan hebben.  
**Details**: Ter bescherming van een service die wordt uitgevoerd op Microsoft Azure, moet u een goed begrip van de toepassingsarchitectuur van uw hebt en zich richten op de [vijf pijlers van softwarekwaliteit](https://docs.microsoft.com/azure/architecture/guide/pillars). Typische verkeer volumes, moet u weten het model connectiviteit tussen de toepassing en andere toepassingen en de service-eindpunten die zichtbaar zijn voor het openbare internet.

Ervoor te zorgen dat een toepassing is flexibel genoeg om af te handelen van een denial of service die gericht op de toepassing zelf het belangrijkste is. Beveiliging en privacy zijn ingebouwd in het Azure-platform, vanaf de [Security Development Lifecycle (SDL)](https://www.microsoft.com/en-us/sdl). De SDL-adressen van beveiliging in elke ontwikkelingsfase en zorgt ervoor dat Azure voortdurend bijgewerkt wordt zodat deze nog beter beveiligen.

**Beste**: Uw toepassingen te ontwerpen [horizontaal schalen](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) om te voldoen aan de vraag van een versterkt belasting, specifiek in het geval van een DDoS-aanval. Als uw toepassing afhankelijk van één exemplaar van een service is, maakt het een single point of failure. Meerdere exemplaren wordt ingericht, kunt u uw systeem beter bestand is tegen en beter schaalbaar.  
**Details**: Voor [Azure App Service](../app-service/app-service-value-prop-what-is.md), selecteer een [App Service-plan](../app-service/overview-hosting-plans.md) die meerdere exemplaren biedt.

Voor Azure Cloud Services, configureert u elk van de rollen gebruiken [meerdere exemplaren](../cloud-services/cloud-services-choose-me.md).

Voor [Azure Virtual Machines](../virtual-machines/windows/overview.md), zorg ervoor dat uw VM-architectuur meer dan één virtuele machine bevat en dat elke virtuele machine wordt opgenomen in een [beschikbaarheidsset](../virtual-machines/virtual-machines-windows-manage-availability.md). Het is raadzaam om voor automatisch te schalen met behulp van de virtuele-machineschaalset ingesteld.

**Beste**: Stapelen beveiligingen in een toepassing vermindert de kans dat een geslaagde aanval. Beveiligde ontwerpen voor uw toepassingen implementeren met behulp van de ingebouwde mogelijkheden van het Azure-platform.  
**Details**: Het risico van aanvallen neemt toe met de grootte (gebied) van de toepassing. U kunt de surface area beperken door met behulp van opname in de whitelist moeten sluiten de beschikbaar gemaakte IP-adresruimte en poorten die nodig zijn niet op de load balancers te hebben geluisterd ([Azure Load Balancer](../load-balancer/load-balancer-get-started-internet-portal.md) en [Azure Application Gateway](../application-gateway/application-gateway-create-probe-portal.md)).

[Netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) zijn van een andere manier om de kwetsbaarheid voor aanvallen verminderen. U kunt [servicetags](../virtual-network/security-overview.md#service-tags) en [toepassingsbeveiligingsgroepen](../virtual-network/security-overview.md#application-security-groups) complexiteit voor het maken van beveiligingsregels en netwerkbeveiliging configureren als een natuurlijk verlengstuk van de structuur van een toepassing te minimaliseren.

U moet implementeren in Azure-services een [virtueel netwerk](../virtual-network/virtual-networks-overview.md) indien mogelijk. Met deze procedure kunt service-resources om te communiceren via privé-IP-adressen. Azure-serviceverkeer vanaf een virtueel netwerk maakt gebruik van openbare IP-adressen als bron-IP-adressen standaard.

Met behulp van [service-eindpunten](../virtual-network/virtual-network-service-endpoints-overview.md) switches service verkeer voor het gebruik van privéadressen van virtuele netwerken als de bron-IP-adressen wanneer ze toegang wilt de Azure-service vanuit een virtueel netwerk tot krijgen.

Vaak zien we klanten on-premises bronnen ophalen aangevallen, samen met hun bronnen in Azure. Als u een on-premises omgeving verbinding naar Azure, minimaliseert u de blootstelling van on-premises bronnen met het openbare internet.

Azure heeft twee DDoS [serviceaanbiedingen](../virtual-network/ddos-protection-overview.md) die bescherming tegen netwerkaanvallen bieden:

- Basisbeveiliging is geïntegreerd in Azure standaard zonder extra kosten. De schaal en de capaciteit van de wereldwijd geïmplementeerde Azure-netwerk biedt beveiliging tegen veelvoorkomende netwerklaag aanvallen via verkeer altijd bewakings- en realtime risicobeperking. Basic vereist geen gebruiker-configuratie of de toepassing wijzigingen en beschermt alle Azure-services, met inbegrip van PaaS-services zoals Azure DNS.
- Standard bescherming biedt geavanceerde mogelijkheden voor DDoS risicobeperking tegen netwerkaanvallen. Het automatisch afgestemd op ter bescherming van uw specifieke Azure-resources. Beveiliging is heel eenvoudig om in te schakelen tijdens het maken van virtuele netwerken. Het kan ook worden uitgevoerd na het maken en vereist geen wijzigingen toepassing of resource.

## <a name="next-steps"></a>Volgende stappen
Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

De volgende resources zijn beschikbaar, voor meer algemene informatie over Azure-beveiliging en gerelateerde Microsoft-services:
* [Azure-Team Beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/) - voor up-to-date informatie over het laatste nieuws over Azure-beveiliging
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - hier Microsoft beveiligingsproblemen, met inbegrip van problemen met Azure, kunnen u ook via e-mail secure@microsoft.com
