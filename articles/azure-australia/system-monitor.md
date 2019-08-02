---
title: Systeem bewaking voor beveiliging in azure Australië
description: Richt lijnen voor het configureren van systeem bewaking binnen de Australische regio's om te voldoen aan de specifieke vereisten van het beleid voor de Australische overheid, de regelgeving en de wetgeving.
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: b68bdb6d1c349df67943889b511eac87c940b531
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571885"
---
# <a name="system-monitoring-for-security-in-azure-australia"></a>Systeem bewaking voor beveiliging in azure Australië

Het gebruik van robuuste beveiligings strategieën die realtime-bewakings-en routine beveiligings beoordelingen omvatten, zijn van cruciaal belang om de dagelijkse operationele beveiliging van uw IT-omgevingen, inclusief Cloud, te verbeteren.

Cloud beveiliging is een gezamenlijke inspanning tussen de klant en de Cloud provider. Er zijn vier services die Microsoft Azure biedt om deze vereisten te vereenvoudigen met betrekking tot de aanbevelingen die zijn opgenomen in de [ACSC-informatie (Information Security Manual Control) van het Australische Cyber Security Center](https://acsc.gov.au/infosec/ism/index.htm) (ISM), met name de implementatie van gecentraliseerde gebeurtenis logboek registratie, controle van gebeurtenis logboeken en het evalueren en beheren van beveiligings lekken. De Microsoft Azure Services zijn:

* Azure Security Center
* Azure Monitor
* Azure Advisor
* Azure-beleid

De ACSC raadt u aan deze services te gebruiken voor **beveiligde** gegevens. Met deze services kunt u uw IT-omgevingen proactief controleren en analyseren, en weloverwogen beslissingen nemen over de manier waarop u resources het beste kunt toewijzen om uw beveiliging te verbeteren. Elk van deze services maakt deel uit van een gecombineerde oplossing om u het beste inzicht, aanbevelingen en beveiliging mogelijk te maken.

## <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) biedt een uniforme beveiligings beheer console die u gebruikt om de beveiliging van Azure-resources en uw gehoste gegevens te controleren en te verbeteren. Azure Security Center omvat een beveiligde Score, een score op basis van een analyse van de status van best practice configuratie van Azure Advisor en de algehele naleving van Azure Policy.

Azure Security Center biedt Azure-klanten de volgende functies:

* Beveiligingsbeleid, evaluatie en aanbevelingen
* Verzameling van beveiligingsgebeurtenissen en zoeken
* Toegangs-en toepassings besturings elementen
* Geavanceerde detectie van bedreigingen
* Just-in-time Virtual Machines Access Control
* Hybride beveiliging

Het bereik van resources die worden bewaakt door Azure Security Center kan worden uitgebreid met ondersteunde on-premises resources in een hybride cloud omgeving. Dit geldt ook voor on-premises resources die momenteel worden bewaakt door een ondersteunde versie van System Center Operations Manager.

De Security Center-laag ' standaard ' biedt ook cloud-gebaseerde beveiligings controles die vereist zijn voor de [ASD essentiële 8](https://acsc.gov.au/publications/protect/essential-eight-explained.htm). Dit zijn onder andere toepassings white list en beperking van beheerders bevoegdheden via just-in-time-toegang.

### <a name="azure-monitor"></a>Azure Monitor

[Azure monitor](https://docs.microsoft.com/azure/azure-monitor/overview) is de gecentraliseerde logboek registratie oplossing voor alle Azure-resources, inclusief Log Analytics en Application Insights. Er worden twee belang rijke gegevens typen verzameld uit Azure-resources: Logboeken en metrieken. Nadat de gegevens in Azure Monitor zijn verzameld, kunnen ze worden gebruikt door een breed scala aan hulpprogram ma's en voor verschillende doel einden.

![Overzicht van Azure Monitor](media/overview.png)

Azure Monitor ook het Azure-activiteiten logboek bevat. In het SActivity-logboek worden alle gebeurtenissen op abonnements niveau opgeslagen die zich hebben voorgedaan in Azure. Hiermee kunnen Azure-klanten de bewerkingen voor ' wie, wat en wanneer ' achter hun Azure-resources zien. Beide logboek registraties op basis van resources die zijn verzonden naar Azure Monitor en activiteiten logboek gebeurtenissen van Azure kunnen worden geanalyseerd met behulp van de ingebouwde Kusto-query taal. Deze logboeken kunnen vervolgens worden geëxporteerd, worden gebruikt voor het maken van aangepaste Dash boards en weer gaven en zijn geconfigureerd om waarschuwingen en meldingen te activeren.

### <a name="azure-advisor"></a>Azure Advisor

[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview) analyses ondersteunde Azure-bronnen, door het systeem gegenereerde logboek bestanden en huidige resource configuraties binnen uw Azure-abonnement. De analyse die in Azure Advisor wordt gegeven, wordt in realtime gegenereerd en gebaseerd op de aanbevolen procedures van micro soft. Alle ondersteunde Azure-resources die aan uw omgeving worden toegevoegd, worden geanalyseerd en er worden geschikte aanbevelingen gegeven. Azure Advisor aanbevelingen worden onderverdeeld in vier best practice Categorieën:

* Beveiliging
* Hoge beschikbaarheid
* Prestaties
* Kosten

Beveiligings aanbevelingen die worden gegenereerd door Azure Advisor, maken deel uit van de algemene beveiligings analyse die wordt geleverd door Azure Security Center.

De door Azure Advisor verzamelde informatie biedt beheerders de volgende mogelijkheden:

* Inzicht in de configuratie van resources die niet voldoet aan de aanbevolen best practice
* Richt lijnen voor specifieke herstel acties om te verbinden
* Classificaties die aangeven welke herstel acties moeten worden uitgevoerd als een hoge prioriteit

### <a name="azure-policy"></a>Azure-beleid

[Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview) biedt de mogelijkheid om regels toe te passen die de typen Azure-resources en de toegestane configuratie regelen. Het beleid kan worden gebruikt om het maken en configureren van resources te beheren of kan worden gebruikt om configuratie-instellingen in een omgeving te controleren. Deze controle resultaten kunnen worden gebruikt om de basis van herstel activiteiten te vormen. Azure Policy wijkt af van op rollen gebaseerd toegangs beheer (RBAC). Azure Policy wordt gebruikt om resources en hun configuratie te beperken, wordt RBAC gebruikt voor het beperken van bevoegde toegang tot Azure-gebruikers.

Of het specifieke beleid wordt afgedwongen of dat het effect van het beleid wordt gecontroleerd, of de naleving van het beleid doorlopend wordt bewaakt en dat er algemene en klantspecifieke nalevings gegevens aan beheerders worden verstrekt. Azure Policy compatibiliteits gegevens worden door gegeven aan Azure Security Center en vormen een onderdeel van de beveiligde Score.

## <a name="key-design-considerations"></a>Belangrijkste overwegingen bij het ontwerpen

Bij het implementeren van een gebeurtenis logboek strategie markeert de ACSC-ISM de volgende overwegingen:

* Centrale logboek registratie voorzieningen
* Specifieke gebeurtenissen die moeten worden geregistreerd
* Beveiliging van gebeurtenis logboeken
* Bewaar periode van gebeurtenis logboek
* Controle van gebeurtenis logboeken

In het geval van het verzamelen en beheren van Logboeken, raadt de ISM ook de routine matige evaluatie van beveiligings problemen van de IT-omgeving van een organisatie aan.

### <a name="centralised-logging"></a>Gecentraliseerde logboek registratie

Elke registratie oplossing moet, waar mogelijk, vastgelegde Logboeken in één gegevens opslagplaats consolideren. Hierdoor wordt niet alleen de operationele complexiteit verminderd en wordt voor komen dat meerdere gegevens silo's worden gemaakt, kunnen gegevens die uit meerdere bronnen worden verzameld samen worden geanalyseerd, zodat eventuele gerelateerde gebeurtenissen kunnen worden geïdentificeerd. Dit is essentieel voor het detecteren en beheren van het bereik van alle Cyber-beveiligings incidenten.

Aan deze eis wordt voldaan voor alle Azure-klanten met Azure Monitor. Deze aanbieding biedt niet alleen een centrale logboek registratie voor opslag in azure voor alle Azure-resources. Daarnaast kunt u hiermee uw gegevens streamen naar een Azure Event hub. Azure Event Hubs biedt een volledig beheerde, realtime Service voor gegevens opname. Als Azure Monitor gegevens worden gestreamd naar een Azure Event hub, kunnen de gegevens ook eenvoudig worden verbonden met bestaande ondersteunde SIEM-opslag plaatsen (Security Information and Event Management) en aanvullende controle hulpprogramma's van derden.

Micro soft biedt ook een eigen Azure native SIEM-oplossing, Azure Sentinel. Azure Sentinel ondersteunt een groot aantal verschillende gegevens connectors en kan worden gebruikt voor het bewaken van beveiligings gebeurtenissen in een hele onderneming. Door de gegevens van ondersteunde [gegevens connectors](https://docs.microsoft.com/azure/sentinel/connect-data-sources)te combi neren, de ingebouwde machine learning van Azure Sentinel en de Kusto-query taal, worden beveiligings beheerders geboden met één oplossing voor waarschuwings detectie, zicht baarheid van bedreigingen, proactieve jacht, en reactie op bedreigingen. Sentinel voorziet ook in een jacht-en notebook-functie waarmee beveiligings beheerders alle stappen kunnen vastleggen die worden uitgevoerd als onderdeel van een beveiligings onderzoek in een herstelbaar Playbook dat kan worden gedeeld binnen een organisatie. Beveiligings beheerders kunnen zelfs de ingebouwde [gebruikers analyse](https://docs.microsoft.com/azure/sentinel/user-analytics) gebruiken om de acties van een enkele benoemde gebruiker te onderzoeken.

### <a name="logged-events-and-log-detail"></a>Geregistreerde gebeurtenissen en logboek Details

De ISM bevat een gedetailleerde lijst met gebeurtenis logboek typen die in elke registratie strategie moeten worden opgenomen. Vastgelegde logboeken moeten voldoende details bevatten om praktisch gebruik te kunnen maken van het uitvoeren van analyses en onderzoeken.

De logboeken die in azure zijn verzameld, vallen onder een van de volgende drie categorieën:

* **Controle-en beheer logboeken**: Deze logboeken bevatten informatie over Azure Resource Manager CREATE-, UPDATE-en DELETE-bewerkingen.

* **Logboeken voor gegevens vlak**: Deze bevatten gebeurtenissen die zijn opgetreden als onderdeel van het gebruik van Azure-resources. Bevat bronnen zoals Windows-gebeurtenis logboeken, waaronder systeem-, beveiligings-en toepassings Logboeken.

* **Verwerkte gebeurtenissen**: Deze gebeurtenissen bevatten informatie over gebeurtenissen en waarschuwingen die automatisch door Azure zijn verwerkt namens de klant. Een voor beeld van een verwerkte gebeurtenis is een Azure Security Center waarschuwing.

De bewaking van virtuele Azure-machines wordt verbeterd door de implementatie van de Virtual Machine-agent voor Windows en Linux. Dit markeert aanzienlijk de omvang van de verzamelde logboek gegevens. De implementatie van deze agent kan worden geconfigureerd om automatisch te worden uitgevoerd via de Azure Security Center.

Micro soft biedt gedetailleerde informatie over Azure-resource-specifieke logboeken en hun [schema's](https://docs.microsoft.com/azure/security/azure-log-audit).

### <a name="log-retention-and-protection"></a>Bewaren en beveiligen van logboek

 Gebeurtenis logboeken moeten veilig worden opgeslagen voor de vereiste Bewaar periode. De ISM adviseert dat Logboeken mini maal zeven jaar worden bewaard. Azure biedt een aantal middelen om de lange levens duur van uw verzamelde logboeken te garanderen. De Azure-logboek gebeurtenissen worden standaard opgeslagen voor 90 dagen. Logboek gegevens die door Azure Monitor worden vastgelegd, kunnen worden verplaatst en opgeslagen op een Azure Storage account dat vereist is voor een lange termijn retentie. Activiteiten logboeken die zijn opgeslagen op een Azure Storage-account, kunnen worden bewaard gedurende een bepaalde periode of voor onbepaalde tijd, indien nodig.

Azure Storage accounts die worden gebruikt voor het opslaan van Azure-logboek gebeurtenissen kunnen geografisch redundant worden gemaakt en er kan een back-up van worden gemaakt met behulp van Azure Backup. Zodra de back-ups van de logboeken zijn vastgelegd door Azure Backup, moet de beheerder goed keuring hebben en moeten back-ups die zijn gemarkeerd voor verwijdering, nog steeds gedurende 14 dagen worden bewaard. Azure Backup maakt 9999 kopieën van een beveiligd exemplaar mogelijk, waardoor er meer dan 27 jaar back-ups worden gemaakt.

Op rollen gebaseerd Access Control moet worden gebruikt voor het beheren van de toegang tot resources die worden gebruikt voor Azure-logboek registratie. Azure Monitor, Azure Storage-accounts en Azure-back-ups moeten worden geconfigureerd met toegangs beheer op basis van rollen om te zorgen voor beveiliging van de gegevens in de logboeken.

### <a name="log-auditing"></a>Logboek controle

De werkelijke waarde van Logboeken wordt gerealiseerd zodra ze worden geanalyseerd. Door zowel een automatische als hand matige analyse te gebruiken en bekend te zijn met de beschik bare hulpprogram ma's, helpt u bij het detecteren en beheren van inbreuken op organisatie-beveiligings beleid en Cyber beveiligings incidenten. Azure Monitor biedt een uitgebreide set hulpprogram ma's voor het analyseren van verzamelde Logboeken. Het resultaat van deze analyse kan vervolgens worden gedeeld tussen systemen, visualised of verspreid in meerdere indelingen.

Logboek gegevens die zijn opgeslagen in Azure Monitor worden bewaard in een Log Analytics-werk ruimte. Alle analyse begint met een query. Azure Monitor query's worden geschreven in de query taal Kusto. Query's vormen de basis van alle uitvoer van Azure Monitor, van Azure-Dash boards tot waarschuwings regels.

![Overzicht van Azure-logboek Query's](media/queries-overview.png)

Controle van logboeken kan worden uitgebreid met behulp van bewakings oplossingen. Dit zijn vooraf verpakte oplossingen die verzamelings logica, query's en gegevens visualisatie weergaven bevatten. Micro soft [biedt](https://docs.microsoft.com/azure/azure-monitor/insights/solutions-inventory) een aantal bewakings oplossingen en aanvullende oplossingen van product leveranciers die u kunt vinden op de Azure Marketplace.

### <a name="vulnerability-assessment-and-management"></a>Beoordeling en beheer van beveiligings problemen

De ISM-opmerkingen die de evaluatie en het beheer van de routine voor het beveiligings beleid zijn essentieel. Uw IT-omgeving is voortdurend in ontwikkeling en de externe beveiligings bedreiging is oneindig gewijzigd. Met Azure Security Center kunt u geautomatiseerd evaluatie van beveiligings problemen uitvoeren en richt lijnen krijgen over het plannen en uitvoeren van herstel activiteiten.

Met een beveiligde Score in Azure Security Center krijgt u een lijst met aanbevelingen die, wanneer toegepast, de beveiliging van uw omgeving wordt verbeterd. De lijst wordt gesorteerd op basis van de impact op de algehele beveiligde Score van hoog naar laag. Door de lijst op impact te best Ellen, kunt u zich richten op de aanbevelingen met de hoogste prioriteit die de meeste waarde bieden bij het verbeteren van uw veiligheid.

Azure Policy speelt ook een belang rijke rol in de doorlopende evaluatie van het beveiligingslek. De typen beleid die beschikbaar zijn in Azure Policy bereik van het afdwingen van resource Tags en-waarden, het beperken van de Azure-regio's waarin resources kunnen worden gemaakt, om het maken van bepaalde resource typen te blok keren. Een set van Azure-beleid kan worden gegroepeerd in initiatieven. Initiatieven worden gebruikt voor het Toep assen van gerelateerde Azure-beleids regels die, wanneer ze samen worden toegepast als groep, de basis vormen van een specifiek beveiligings-of nalevings doel.

Azure Policy heeft een bibliotheek met ingebouwde beleids definities die voortdurend groeien. Azure Portal biedt u ook de mogelijkheid om uw eigen aangepaste Azure Policy definities te maken. Wanneer u een beleid in de bestaande bibliotheek hebt gevonden of een nieuwe hebt gemaakt, kunt u het beleid toewijzen aan Azure-resources. Deze toewijzingen kunnen op verschillende niveaus in de resource beheer [hiërarchie worden toegepast](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage) . Beleids toewijzing is overgenomen, wat betekent dat alle onderliggende resources binnen een bereik dezelfde beleids toewijzing ontvangen. Resources kunnen ook worden uitgesloten van de toewijzing van het bereik beleid zoals vereist.

Alle geïmplementeerde Azure-beleids regels dragen bij aan de beveiligde Score van een organisatie. In een zeer Bespoke omgeving kunnen aangepaste definities van Azure Policy worden gemaakt en geïmplementeerd om controle gegevens op te geven die zijn afgestemd op specifieke werk belastingen.

## <a name="getting-started"></a>Aan de slag

Micro soft raadt de volgende eerste stappen aan om te beginnen met Azure Security Center en optimaal gebruik te maken van Azure Monitor, Advisor en het beleid:

* Azure Security Center inschakelen
* Upgraden naar de Standard-laag
* Automatische inrichting van micro soft Monitoring Agent op ondersteunde Azure-Virtual Machines inschakelen
* De beveiligings aanbevelingen en waarschuwingen in het dash board van het Security Center controleren, prioriteiten geven en beperken

## <a name="next-steps"></a>Volgende stappen

Lees [Azure Policy en Azure-blauw drukken](azure-policy.md) voor meer informatie over het implementeren van governance en controle over uw Azure Australia-resources om te zorgen voor naleving van beleid en regelgeving.
