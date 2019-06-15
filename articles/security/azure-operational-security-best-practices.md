---
title: Aanbevolen beveiligingsprocedures voor het beveiligen van uw activa - Microsoft Azure
description: Dit artikel bevat een set van aanbevolen procedures voor het beveiligen van uw gegevens, toepassingen en andere assets in Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/06/2019
ms.author: terrylan
ms.openlocfilehash: 0418d325f3b3719549181a48fc0432a677f695d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65795893"
---
# <a name="azure-operational-security-best-practices"></a>Azure best practices voor operationele beveiliging
Dit artikel bevat een set van aanbevolen procedures voor het beveiligen van uw gegevens, toepassingen en andere assets in Azure.

De best practices zijn gebaseerd op een consensus van advies, en ze werken met de huidige mogelijkheden van Azure-platform en functie ingesteld. Adviezen en technologieën verloop van tijd veranderen en in dit artikel wordt bijgewerkt op gezette tijden op basis van deze wijzigingen.

## <a name="define-and-deploy-strong-operational-security-practices"></a>Definiëren en implementeren van procedures voor sterke operationele beveiliging
Azure operationele beveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere assets in Azure. Operationele beveiliging in Azure is gebouwd op een framework waarin de kennis opgedaan uit mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl), wordt de [Microsoft Security Response Center](https://www.microsoft.com/msrc?rtc=1) programma en een diep besef van het landschap van cyberveiligheidsbedreigingen.

## <a name="manage-and-monitor-user-passwords"></a>Beheren en controleren van wachtwoorden van gebruikers
De volgende tabel bevat enkele aanbevolen procedures met betrekking tot het beheren van wachtwoorden van gebruikers:

**Beste**: Zorg ervoor dat u het juiste niveau van beveiliging met een wachtwoord hebt in de cloud.   
**Details**: Volg de instructies in [Microsoft Password Guidance](https://www.microsoft.com/research/publication/password-guidance/), die is afgestemd op gebruikers van de Microsoft identity-platforms (Azure Active Directory, Active Directory en de Microsoft-account).

**Beste**: Monitor voor de verdachte activiteit met betrekking tot uw gebruikersaccounts.   
**Details**: Controleren op [gebruikers die risico lopen](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk) en [riskante aanmeldingen](../active-directory/reports-monitoring/concept-risk-events.md) met behulp van Azure AD-beveiligingsgroep rapporten.

**Beste**: Automatisch detecteren en herstellen van wachtwoorden met een hoog risico.   
**Details**: [Azure AD Identity Protection](../active-directory/identity-protection/overview.md) is een functie van de Azure AD Premium P2-editie waarmee u kunt:

- Detecteren van mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie
- Automatische antwoorden op gedetecteerde verdachte activiteit die betrekking op de identiteiten van uw organisatie hebben configureren
- Verdachte incidenten onderzoeken en passende maatregelen op te lossen

## <a name="receive-incident-notifications-from-microsoft"></a>Incident meldingen ontvangen van Microsoft
Zorg ervoor dat uw beveiligingsteam operations Azure incident meldingen ontvangt van Microsoft. Een incident melding kunt uw beveiligingsteam kent u Azure-resources zijn aangetast, zodat ze snel kunnen reageren op en herstellen van mogelijke beveiligingsrisico's.

In de portal voor Azure-inschrijving, kunt u ervoor zorgen dat contactgegevens van beheerder bevat informatie die op de hoogte beveiligingsbewerkingen stellen. Contactgegevens is een e-mailadres en telefoonnummer getal.

## <a name="organize-azure-subscriptions-into-management-groups"></a>Azure-abonnementen in beheergroepen organiseren
Als uw organisatie veel abonnementen heeft, moet u mogelijk een manier om efficiënt beheer van toegang, beleid en naleving voor deze abonnementen. [Azure-beheergroepen](../governance/management-groups/create.md) bieden een bereik dat hoger is dan de abonnementen. U abonnementen in containers beheergroepen genoemd ordenen en de voorwaarden van uw beheeracties toepassen op de beheergroepen. Alle abonnementen in een beheergroep nemen automatisch de voorwaarden over die op de beheergroep zijn toegepast.

U kunt een flexibele structuur van beheergroepen en -abonnementen in een map op te bouwen. Elke directory is een op het hoogste niveau de beheergroep de root management-groep met de naam gegeven. Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn opgevouwen. De root management-groep kan globaal beleid en RBAC-toewijzingen moet worden toegepast op het niveau van de map.

Hier volgen enkele aanbevolen procedures voor het gebruik van beheergroepen:

**Beste**: Zorg ervoor dat nieuwe abonnementen governance-elementen, zoals beleidsregels en machtigingen toepassen wanneer ze worden toegevoegd.   
**Details**: Gebruik de root management-groep om toe te wijzen bedrijfsbrede security-elementen die betrekking hebben op alle Azure-assets. Beleidsregels en machtigingen zijn voorbeelden van elementen.

**Beste**: Lijn de hoogste niveaus van van beheergroepen met segmentatie strategie voor een punt voor controle en beleid consistentie binnen elk segment.   
**Details**: Maak één beheergroep voor elk segment onder de root management-groep. Maak geen andere beheergroepen onder de hoofdmap.

**Beste**: Diepte van de management-groep om verwarring die zowel operations en beveiliging belemmert te beperken.   
**Details**: Beperk uw hiërarchie op drie niveaus, met inbegrip van de hoofdmap.

**Beste**: Zorgvuldig selecteren welke items moeten betrekking hebben op de hele onderneming de root management-groep.   
**Details**: Controleer of root management-groepselementen hebben een duidelijke moet worden toegepast op elke resource en dat ze weinig impact.

Goede kandidaten zijn onder andere:

- Wettelijke eisen die waarvoor een duidelijke bedrijfsimpact (bijvoorbeeld beperkingen met betrekking tot gegevenssoevereiniteit)
- Vereisten met vrijwel geen potentiële negatieve invloed op bewerkingen, zoals het beleid met de kracht van de controle of RBAC machtigingsniveaus die goed zijn gecontroleerd

**Beste**: Zorgvuldig plannen en alle bedrijfsbrede wijzigingen op de root management-groep testen voordat u deze (beleid, RBAC-model, enzovoort) toepast.   
**Details**: Wijzigingen in de root management-groep kunnen invloed hebben op elke resource in Azure. Hoewel ze een krachtige manier om te zorgen voor consistentie binnen de hele organisatie bieden, kan fouten of onjuiste syntaxis negatief beïnvloeden productiebewerkingen. Alle wijzigingen in de root management-groep testen in een testomgeving of productie test.

## <a name="streamline-environment-creation-with-blueprints"></a>Maken van de omgeving stroomlijnen met blauwdrukken
[De Azure-blauwdrukken](../governance/blueprints/overview.md) service kunt cloudarchitecten en -groepen van centrale informatie technologie voor het definiëren van een herhaalbare set Azure-resources die worden geïmplementeerd en voldoet aan de normen, patronen en vereisten van een organisatie. Azure blauwdrukken maakt het mogelijk voor ontwikkelteams om snel te bouwen en een paar nieuwe omgevingen met een set van de ingebouwde onderdelen en het vertrouwen dat ze deze omgevingen binnen de organisatie-nalevingsbeleid maakt.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Storage-services controleren voor onverwachte wijzigingen in werking
Vaststellen en oplossen van problemen in een gedistribueerde toepassing die wordt gehost in een cloudomgeving kunnen zijn complexer dan in traditionele-omgevingen. Toepassingen kunnen worden geïmplementeerd in een PaaS of IaaS-infrastructuur, on-premises, op een mobiel apparaat, of in een combinatie van deze omgevingen. Netwerkverkeer van uw toepassing kan passeren openbare en particuliere netwerken en toepassingen kan gebruikmaken van meerdere opslagtechnologieën.

De storage-services die uw toepassing wordt gebruikt voor eventuele onverwachte wijzigingen in gedrag (zoals tragere responstijden), moet u continu controleren. Logboekregistratie gebruiken om meer gedetailleerde gegevens te verzamelen en analyseren van een probleem in de diepte. De diagnostische gegevens die u via controle en logboekregistratie aanschaft helpt u bij het bepalen van de hoofdoorzaak van het probleem dat uw toepassing is opgetreden. U kunt vervolgens het probleem oplossen met en bepaal de juiste stappen worden opgelost.

[Azure Storage Analytics](../storage/storage-analytics.md) logboekregistratie uitvoert en metrische gegevens biedt voor Azure storage-account. U wordt aangeraden dat u deze gegevens gebruikt om aanvragen te traceren, gebruikstrends te analyseren en problemen diagnosticeren met uw storage-account.

## <a name="prevent-detect-and-respond-to-threats"></a>Voorkomen, detecteren en direct reageren op bedreigingen
[Azure Security Center](../security-center/security-center-intro.md) helpt u bij het voorkomen, detecteren, en reageren op bedreigingen dankzij een verhoogde zichtbaarheid (en controle over) de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met verschillende beveiligingsoplossingen te detecteren.

De gratis laag van Security Center biedt beperkte beveiliging voor alleen uw Azure-resources. De prijscategorie Standard biedt deze mogelijkheden ook voor on-premises en andere clouds. Standard van Security Center helpt u bij het zoeken en oplossen van beveiligingsproblemen, toegangs- en toepassingsbesturingselementen schadelijke activiteiten te blokkeren, bedreigingen detecteren met behulp van analyses en gegevens van toepassing en snel reageren bij aanvallen. U kunt Security Center Standard de eerste 60 dagen kosteloos proberen. We raden aan dat u [uw Azure-abonnement upgraden naar Security Center Standard](../security-center/security-center-get-started.md).

Security Center gebruiken voor een centraal overzicht van de beveiligingsstatus van al uw Azure-resources. In een oogopslag controleren of de juiste beveiligingscontroles geïnstalleerd zijn en correct geconfigureerd en snel zien of er resources waarvoor aandacht.

Security Center kan ook worden geïntegreerd met [Windows Defender Advanced Threat Protection (ATP)](../security-center/security-center-wdatp.md), waarmee u uitgebreide functionaliteit voor eindpunt detectie en reactie (EDR). Met Windows Defender ATP-integratie, kunt u afwijkingen herkennen. U kunt ook detecteren en reageren op geavanceerde aanvallen op de servereindpunten bewaakt door Security Center.

Bijna alle enterprise-organisaties hebben een en -gebeurtenissen (SIEM) beheersysteem voor informatiebeveiliging voor het identificeren van opkomende bedreigingen door consolidatie van gegevens uit diverse signaal verzamelen-apparaten. De logboeken worden vervolgens geanalyseerd door een systeem voor analytics voor het identificeren van wat is 'interessante' van de ruis die is onvermijdelijk in alle logboekbestanden te verzamelen en analyseoplossingen.

[Azure Sentinel](../sentinel/overview.md) is een schaalbare, cloud-native, security information en event management (SIEM) en beveiligingsoplossing voor orchestration geautomatiseerd antwoord (ZWEEF). Azure Sentinel biedt intelligente analyses en bedreigingsinformatie beveiligingsinformatie via waarschuwing detectie, zichtbaarheid van de bedreiging, proactieve jacht en geautomatiseerde bedreigingen te kunnen reageren.

Hier volgen enkele aanbevolen procedures voor het voorkomen, detecteren en reageren op bedreigingen:

**Beste**: Verhogen de snelheid en schaalbaarheid van uw SIEM-oplossing met behulp van een cloud-gebaseerde SIEM.   
**Details**: De functies en mogelijkheden van onderzoeken [Azure Sentinel](../sentinel/overview.md) en deze te vergelijken met de mogelijkheden van wat u momenteel bent met on-premises. Overweeg de overstap naar Azure Sentinel als deze voldoet aan vereisten van de SIEM van uw organisatie.

**Beste**: Het meest ernstige beveiligingsproblemen zoeken, zodat u prioriteiten onderzoek aanbrengen kunt.   
**Details**: Controleer uw [Azure beveiligde score](../security-center/security-center-secure-score.md) om te zien van de aanbevelingen die voortvloeien uit de Azure-beleid en die is ingebouwd in Azure Security Center-initiatieven. Deze aanbevelingen helpen adres grootste risico's, zoals beveiligingsupdates, endpoint protection, versleuteling, beveiligingsconfiguraties, ontbrekende WAF, internet verbonden VM's en nog veel meer.

De beveiligde score, die is gebaseerd op Center voor besturingselementen voor Internet Security (CIS), kunt u de benchmark-test uitvoeren van uw organisatie Azure-beveiliging op basis van externe bronnen. Externe validatie kunt valideren en verrijken van de beveiligingsstrategie van uw team.

**Beste**: Controleer de beveiligingsstatus van machines, netwerken, opslag- en gegevensservices en toepassingen te detecteren en te bepalen welke mogelijke beveiligingsproblemen.  
**Details**: Ga als volgt de [beveiligingsaanbevelingen](../security-center/security-center-recommendations.md) in Security Center starten met de hoogste prioriteit-items.

**Beste**: Waarschuwingen van Beveiligingscentrum integreren met uw security information en event management (SIEM)-oplossing.   
**Details**: De meeste organisaties met een SIEM gebruiken als een centrale clearinghouse voor beveiligingswaarschuwingen waarvoor een analist-antwoord. Verwerkte gebeurtenissen die worden geproduceerd door Security Center worden gepubliceerd naar de Azure-activiteitenlogboek, een van de logboeken die beschikbaar zijn via Azure Monitor. Azure Monitor biedt een geconsolideerde pijplijn voor de routering van uw bewakingsgegevens naar een SIEM-hulpprogramma. Zie [beveiligingsoplossingen integreren in Security Center](../security-center/security-center-partner-integration.md#exporting-data-to-a-siem) voor instructies. Als u Azure Sentinel, Zie [verbinding maken met Azure Security Center](../sentinel/connect-azure-security-center.md).

**Beste**: Integratie van Azure logboeken met uw SIEM.   
**Details**: Gebruik [Azure Monitor om te verzamelen en gegevens exporteren](../azure-monitor/overview.md#integrate-and-export-data). Met deze procedure is van essentieel belang voor het inschakelen van security incident onderzoek en online logboekbehoud is beperkt. Als u Azure Sentinel, Zie [verbinding maken met gegevensbronnen](../sentinel/connect-data-sources.md).

**Beste**: Versnel uw onderzoek en jacht processen en fout-positieven verminderen door het eindpunt detectie en reactie (EDR)-mogelijkheden integreren in uw onderzoek van de aanval.   
**Details**: [Windows Defender ATP-integratie inschakelen](../security-center/security-center-wdatp.md#enable-windows-defender-atp-integration) via uw beveiligingsbeleid voor Security Center. Overweeg het gebruik van Azure Sentinel voor threat jacht en reageren op incidenten.

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
**Details**: U kunt uw Azure DevOps Projects te configureren [automatisch bouwen en implementeren van](https://docs.microsoft.com/azure/devops/pipelines/index?azure-devops) op Azure-web-apps of cloudservices. Azure DevOps implementeert automatisch de binaire bestanden hierna een build naar Azure na elke check code in. Het bouwproces pakket komt overeen met de opdracht pakket in Visual Studio en de publicatie stappen zijn gelijk aan de opdracht Publish in Visual Studio.

**Beste**: Automatiseer release management.  
**Details**: [Azure pijplijnen](https://docs.microsoft.com/azure/devops/pipelines/index?azure-devops) is een oplossing voor de implementatie van meerdere fasen automatiseren en beheren van de release-proces. Beheerde continue implementatie pijplijnen om snel, eenvoudig en vaak vrij te maken. Met Azure-pijplijnen, kunt u uw release-proces automatiseren en u kunt beschikken over vooraf gedefinieerde goedkeuringswerkstromen. Implementeer on-premises en naar de cloud uitbreiden en aanpassen zoals vereist.

**Beste**: Controleer de prestaties van uw app voordat u het starten of updates naar productie implementeren.  
**Details**: Uitvoeren van cloud-gebaseerde [belastingstests](/azure/devops/test/load-test/overview#alternatives) aan:

- Prestatieproblemen in uw app vinden.
- Implementatie-kwaliteit verbeteren.
- Zorg ervoor dat uw app altijd beschikbaar is.
- Zorg ervoor dat uw app kan worden gebruikt voor het afhandelen van verkeer voor uw volgende starten of marketing-campagne.

[Apache JMeter](https://jmeter.apache.org/) is een gratis, populaire open-source-hulpprogramma met een sterke community een back-up.

**Beste**: Bewaak de prestaties van toepassingen.  
**Details**: [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) is een service extensible application performance management (APM) voor webontwikkelaars op meerdere platforms. Application Insights gebruiken om uw livewebtoepassing controleren. Er wordt automatisch gedetecteerd prestatieafwijkingen. Het bevat analytics-hulpprogramma's kunt u problemen identificeren en te begrijpen wat gebruikers daadwerkelijk doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren.

## <a name="mitigate-and-protect-against-ddos"></a>Te beperken en te beveiligen tegen DDoS
Distributed denial of service (DDoS) is een soort aanval waarmee wordt geprobeerd te weinig toepassingsresources. Het doel is van invloed op de beschikbaarheid van de toepassing en de mogelijkheid voor het verwerken van legitieme aanvragen. Deze aanvallen worden steeds meer geavanceerde en grotere in omvang en impact. Ze kunnen worden gericht op een willekeurig eindpunt dat openbaar bereikbaar is via het internet.

Het ontwerpen en bouwen voor DDoS-bescherming vereist plannen en ontwerpen voor tal van foutmodi. Hieronder vindt u aanbevolen procedures voor het bouwen van robuuste DDoS services op Azure.

**Beste**: Zorg ervoor dat beveiliging een prioriteit gedurende de hele levenscyclus van een toepassing, van ontwerp en implementatie tot implementatie en bewerkingen. Toepassingen kunnen fouten optreden die een relatief laag volume aan aanvragen voor het gebruik van een groot aantal bronnen, wat resulteert in een serviceonderbreking toestaan hebben.  
**Details**: Ter bescherming van een service die wordt uitgevoerd op Microsoft Azure, moet u een goed begrip van de toepassingsarchitectuur van uw hebt en zich richten op de [vijf pijlers van softwarekwaliteit](https://docs.microsoft.com/azure/architecture/guide/pillars). Typische verkeer volumes, moet u weten het model connectiviteit tussen de toepassing en andere toepassingen en de service-eindpunten die zichtbaar zijn voor het openbare internet.

Ervoor te zorgen dat een toepassing is flexibel genoeg om af te handelen van een denial of service die gericht op de toepassing zelf het belangrijkste is. Beveiliging en privacy zijn ingebouwd in het Azure-platform, vanaf de [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl). De SDL-adressen van beveiliging in elke ontwikkelingsfase en zorgt ervoor dat Azure voortdurend bijgewerkt wordt zodat deze nog beter beveiligen.

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

## <a name="enable-azure-policy"></a>Azure Policy inschakelen
[Azure Policy](../governance/policy/overview.md) is een service in Azure die u gebruiken om te maken, toewijzen en beheren van beleid. Deze beleidsregels afdwingen regels en effecten voor uw resources, zodat deze resources voldoen aan uw bedrijfsnormen en service level agreements blijven. Azure Policy voorziet in deze behoefte door uw resources met toegewezen beleid te controleren op niet-naleving.

Azure-beleid om te controleren en afdwingen schriftelijke beleid van uw organisatie inschakelt. Dit zorgt ervoor van naleving van uw bedrijf en wettelijke beveiligingsvereisten door beveiligingsbeleid centraal te beheren voor uw hybride cloudworkloads. Meer informatie over het [maken en beheren van beleidsregels voor het afdwingen van naleving](../governance/policy/tutorials/create-and-manage.md). Zie [Azure structuur van beleidsdefinities](../governance/policy/concepts/definition-structure.md) voor een overzicht van de elementen van een beleid.

Hier volgen enkele aanbevolen procedures voor beveiliging te volgen nadat u Azure Policy nemen:

**Beste**: Beleid ondersteunt verschillende soorten effecten. U kunt meer informatie over deze in [Azure structuur van beleidsdefinities](../governance/policy/concepts/definition-structure.md#policy-rule). Zakelijke activiteiten kunnen negatief worden beïnvloed door de **weigeren** effect en de **herstellen** zijn doorgevoerd, dus beginnen met de **controleren** effect te beperken van het risico van negatieve invloed uit het beleid.   
**Details**: [Voor beleidsimplementaties in de controlemodus Start](../governance/policy/concepts/definition-structure.md#policy-rule) en vervolgens later de voortgang naar **weigeren** of **herstellen**. Testen en de resultaten van de audit-effect evalueren voordat u met verdergaat **weigeren** of **herstellen**.

Zie voor meer informatie, [maken en beheren van beleidsregels voor het afdwingen van naleving](../governance/policy/tutorials/create-and-manage.md).

**Beste**: Identificeren van de rollen die verantwoordelijk is voor de bewaking van beleidsschendingen en ervoor te zorgen dat de juiste herstelactie snel wordt uitgevoerd.   
**Details**: Hebt u de naleving van de toegewezen rol bewaken via de [Azure-portal](../governance/policy/how-to/get-compliance-data.md#portal) of via de [vanaf de opdrachtregel](../governance/policy/how-to/get-compliance-data.md#command-line).

**Beste**: Azure Policy is een technische weergave van een organisatie schriftelijke beleidsregels. Alle Azure-beleid worden toegewezen aan het organisatiebeleid voorkoming van verwarring en consistentie vergroten.   
**Details**: De toewijzing van het document in de documentatie van uw organisatie of in de Azure policy zelf door een verwijzing naar het organisatiebeleid toe te voegen in de Azure [beschrijving van het beleid](../governance/policy/concepts/definition-structure.md#display-name-and-description) of de Azure policy [initiatief](../governance/policy/concepts/definition-structure.md#initiatives) de beschrijving.

## <a name="monitor-azure-ad-risk-reports"></a>Monitor voor Azure AD risico lopen
De meeste van de beveiliging terugdringen plaatsvinden als aanvallers toegang krijgen tot een omgeving door het stelen van de identiteit van een gebruiker. Verdachte identiteiten detecteren, is geen eenvoudige taak. Azure AD maakt gebruik van adaptieve machine learning-algoritmes en -methodieken voor het detecteren van verdachte activiteit die is gekoppeld aan uw gebruikersaccounts. Elke gedetecteerde verdachte actie wordt opgeslagen in een record met de naam een [risicogebeurtenis](../active-directory/reports-monitoring/concept-risk-events.md). Risicogebeurtenissen worden geregistreerd in Azure AD-beveiligingsgroep rapporten. Lees voor meer informatie over de [gebruikers lopen risico beveiligingsrapport](../active-directory/reports-monitoring/concept-user-at-risk.md) en de [riskante aanmeldingen beveiligingsrapport](../active-directory/reports-monitoring/concept-risky-sign-ins.md).

## <a name="next-steps"></a>Volgende stappen
Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

De volgende resources zijn beschikbaar, voor meer algemene informatie over Azure-beveiliging en gerelateerde Microsoft-services:
* [Azure-Team Beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/) - voor up-to-date informatie over het laatste nieuws over Azure-beveiliging
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - hier Microsoft beveiligingsproblemen, met inbegrip van problemen met Azure, kunnen u ook via e-mail secure@microsoft.com
