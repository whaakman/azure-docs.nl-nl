---
title: Azure bedrijfsbeveiliging | Microsoft Docs
description: Meer informatie over de Microsoft Operations Management Suite (OMS), de services en hoe het werkt.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: 8528eeb4eac8397beaa3d0dca37ce9eb33167b32
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="azure-operational-security"></a>Azure-operationele beveiliging
## <a name="introduction"></a>Inleiding

### <a name="overview"></a>Overzicht
We weten dat beveiliging taak in de cloud en hoe belangrijk is dat u tijdig en informatie over Azure-beveiliging vinden. Een van de beste redenen voor het gebruik van Azure voor uw toepassingen en services is om te profiteren van de grote verscheidenheid aan beveiligingsprogramma's en mogelijkheden die beschikbaar. Deze hulpprogramma's en mogelijkheden te maken het mogelijk om veilige oplossingen maken op de beveiligde Azure-platform. Windows Azure moet vertrouwelijkheid, integriteit en beschikbaarheid van klantgegevens, maar ook transparante accountability opgeven.

Biedt een uitgebreide blik op de operationele beveiliging met Windows Azure zodat klanten beter inzicht krijgen de matrix van beveiligingsmechanismen geïmplementeerd in Microsoft Azure van zowel de klant en Microsoft operationele perspectieven dit witboek 'Azure operationele beveiliging', die is geschreven.

### <a name="azure-platform"></a>Azure-Platform
Azure is een openbare cloud service-platform die ondersteuning biedt voor een brede selectie van besturingssystemen, programmeertalen, frameworks, hulpprogramma's, databases, en apparaten. Linux-containers kan worden uitgevoerd met Docker-integratie; bouwen van apps met JavaScript, Python, .NET, PHP, Java en Node.js; Build back-ends voor iOS, Android en Windows apparaten. Azure cloudservice ondersteunt dezelfde technologieën miljoenen ontwikkelaars en IT-professionals die al zijn afhankelijk van en vertrouwt.

Wanneer u bouwen op of IT-activa te migreren, een openbare cloud-serviceprovider u vertrouwen op de mogelijkheden voor uw toepassingen en gegevens beschermen met de services en de besturingselementen die organisatie ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

Azure infrastructuur van de faciliteit is ontworpen voor het hosten van miljoenen klanten tegelijkertijd van toepassingen en biedt een betrouwbare basis waarop bedrijven kunnen voldoen aan de beveiligingsvereisten. Bovendien biedt Azure u een breed scala aan configureerbare beveiligingsopties en de mogelijkheid om ze te beheren zodat u kunt de beveiliging om te voldoen aan de unieke vereisten van uw organisatie implementaties aanpassen. Dit document wordt helpt u begrijpen hoe Azure-beveiliging mogelijkheden kunt u aan deze vereisten voldoen.

### <a name="abstract"></a>Abstracte
Azure bedrijfsbeveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere elementen in Microsoft Azure. Azure operationele beveiliging is gebaseerd op een framework dat de kennis die is opgedaan met verschillende mogelijkheden die uniek voor Microsoft zijn, waaronder Microsoft Security Development Lifecycle (SDL), de Microsoft Security Response Center-programma en grondige kennis van de threat cybersecurity liggend opgenomen.

Dit document bevat een overzicht van de aanpak van Microsoft Azure operationele beveiliging binnen de Microsoft Azure cloud-platform en omvat volgende services:
1.  [Azure Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure-netwerk-watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-operations-management-suite"></a>Microsoft Operations Management Suite

Microsoft Operations Management Suite (OMS) kan de IT-beheeroplossing voor de hybride cloud. Alleen wordt gebruikt of als u wilt uitbreiden van uw bestaande System Center-implementatie, OMS biedt u de maximale flexibiliteit en controle voor cloud-gebaseerde beheer van uw infrastructuur.

![Microsoft Operations Management Suite](./media/azure-operational-security/azure-operational-security-fig1.png)

Met OMS, kunt u een instantie in een cloud, met inbegrip van lokale, Azure, AWS, Windows Server, Linux, VMware en OpenStack, tegen lagere kosten dan concurrerende oplossingen beheren. OMS is gebouwd voor de cloud eerste, biedt een nieuwe benadering voor het beheren van uw onderneming is de snelste, meest rendabele manier om te voldoen aan de nieuwe zakelijke uitdagingen en geschikt voor de nieuwe werkbelastingen, toepassingen en cloudomgevingen.

### <a name="oms-services"></a>OMS-services

De kernfunctionaliteit van OMS wordt geleverd door een reeks services die in Azure worden uitgevoerd. Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen.

| Service  | Beschrijving|
| :------------- | :-------------|
| Log Analytics | Bewaak en analyseer de beschikbaarheid en prestaties van verschillende resources, met inbegrip van fysieke en virtuele machines. |
|Automatisering | Automatiseer handmatige processen en dwing configuraties af voor fysieke en virtuele machines. |
| Back-up | Back-up en herstel van essentiële gegevens. |
| Site Recovery | Bied hoge beschikbaarheid voor kritieke toepassingen. |

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) biedt bewakingsservices voor OMS door in een centrale opslagplaats gegevens te verzamelen van beheerde resources. Deze gegevens kunnen gebeurtenissen, prestatiegegevens en aangepaste gegevens omvatten die via de API worden geleverd. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export.


Deze methode kunt u gegevens uit diverse bronnen consolideren, zodat u kunt combineren gegevens van uw Azure-services met uw bestaande lokale omgeving. De methode maakt ook een duidelijk onderscheid tussen het verzamelen van gegevens en het bewerken hiervan. Zo zijn alle bewerkingen beschikbaar voor alle soorten gegevens.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

De Log Analytics-service beheert uw cloud-gebaseerde gegevens veilig met behulp van de volgende methoden:
-   gegevensscheiding
-   bewaren van gegevens
-   Fysieke beveiliging
-   Incidentbeheer
-   Naleving
-   standaarden veiligheidscertificaten

### <a name="azure-backup"></a>Azure Backup

[Azure Backup](http://azure.microsoft.com/documentation/services/backup) biedt gegevens back-up en herstellen van services en maakt deel uit van de OMS-suite van producten en services.
Het beschermt uw toepassingsgegevens en bewaart deze jarenlang, zonder dat u grote investeringen hoeft te doen en met een minimum aan operationele kosten. Deze kunt een back-ups maken van fysieke en virtuele Windows-servers naast werkbelastingen van toepassingen zoals SQL Server en SharePoint. Kan ook worden gebruikt door [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) beveiligde om gegevens te repliceren naar Azure voor redundantie en langdurige opslag.


Beveiligde gegevens in Azure Backup worden opgeslagen in een back-upkluis in een bepaalde geografische regio. De gegevens binnen dezelfde regio worden gerepliceerd en afhankelijk van het type kluis, kan ook worden gerepliceerd naar een andere regio voor verdere tolerantie.

### <a name="management-solutions"></a>Beheeroplossingen
[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) van Microsoft cloud-gebaseerde IT beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloud-infrastructuur.


[Oplossingen voor](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) voorverpakte sets met logics die een bepaalde beheerscenario met een of meer OMS-services te implementeren. Er zijn verschillende oplossingen beschikbaar van Microsoft en partners die u gemakkelijk kunt toevoegen aan uw Azure-abonnement om de waarde van uw investering in OMS te vergroten. Als partner kunt u uw eigen oplossingen voor de ondersteuning van uw toepassingen en services en ze te bieden aan gebruikers via de snel starten-sjablonen of Azure Marketplace.


![Beheeroplossingen](./media/azure-operational-security/azure-operational-security-fig4.png)

Een goed voorbeeld van een oplossing die meerdere services gebruikt om aanvullende functionaliteit te bieden is de [Update beheeroplossing](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Deze oplossing gebruikt de [logboekanalyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) agent voor Windows en Linux om informatie te verzamelen over de vereiste updates op elke agent. Het schrijft deze gegevens naar de opslagplaats van Log Analytics waar u ze kunt analyseren met een inbegrepen dashboard.

Wanneer u een implementatie van runbooks in maakt [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) worden gebruikt voor het installeren van vereiste updates. U beheert dit hele proces in de portal en hoeft zich geen zorgen te maken over de onderliggende details.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center beschermt u uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. In de service u zich kunt definiëren niet alleen op basis van uw Azure-abonnementen, maar ook het beleid op basis van [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), zodat u gedetailleerder worden uitgevoerd kunt.

### <a name="security-policies-and-recommendations"></a>Beveiligingsbeleid en aanbevelingen

Een beveiligingsbeleid bepaalt welke set besturingselementen wordt aanbevolen voor resources binnen het opgegeven abonnement of de opgegeven resourcegroep.

In Security Center definieert u de beleidsregels op grond van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de gevoeligheid van de gegevens.

![Beveiligingsbeleid en aanbevelingen](./media/azure-operational-security/azure-operational-security-fig5.png)


Beleidsregels die zijn ingeschakeld op het abonnementsniveau automatisch doorgegeven aan alle resourcegroepen binnen het abonnement, zoals weergegeven in het diagram aan de rechterkant:


### <a name="data-collection"></a>Gegevensverzameling

Met Security Center worden gegevens van uw virtuele machines (VM's) verzameld om de beveiligingsstatus van de VM's te beoordelen, aanbevelingen voor beveiliging te geven en u te waarschuwen bij bedreigingen. Wanneer uw eerste toegang Security Center, verzamelen van gegevens is ingeschakeld op alle VM's in uw abonnement. Gegevensverzameling wordt aanbevolen, maar u kunt gegevensverzameling indien gewenst ook uitschakelen in het Security Center-beleid.

### <a name="data-sources"></a>Gegevensbronnen

- Azure Security Center analyseert gegevens uit de volgende bronnen om inzicht in uw beveiligingsstatus te geven, beveiligingsproblemen te identificeren, oplossingen aan te raden en actieve bedreigingen te detecteren:

-   Azure Services: gebruikt informatie over de configuratie van de Azure-services die u hebt geïmplementeerd door te communiceren met de resourceprovider van die service.

- Netwerkverkeer: gebruikt steekproefgewijs netwerkverkeermetagegevens uit de infrastructuur van Microsoft, zoals bron-/doel-IP/poort, pakketgrootte en netwerkprotocol.

-   Oplossingen van partners: gebruikt beveiligingswaarschuwingen van geïntegreerde partneroplossingen, zoals firewalls en antimalwareoplossingen.

-   Uw virtuele machines: gebruikt configuratie-informatie en informatie over beveiligingsgebeurtenissen, zoals Windows-gebeurtenis- en auditlogboeken, IIS-logboeken, syslog-berichten en crashdumpbestanden van uw virtuele machines.

### <a name="data-protection"></a>Gegevensbeveiliging

Om klanten te helpen bedreigingen te voorkomen, te detecteren en erop te reageren, verzamelt en verwerkt Azure Security Center gegevens over beveiliging, zoals configuratie-informatie, metagegevens, gebeurtenislogboeken, crashdumpbestanden en nog veel meer. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service.

-   **Scheiding van gegevens**: gegevens worden op een logische manier apart van elkaar gehouden, in elk onderdeel van de service. Alle gegevens worden gemarkeerd per organisatie. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service.

-   **Toegang tot gegevens**: aanbevelingen voor beveiliging en onderzoeken van mogelijke bedreigingen, medewerkers van Microsoft kunnen toegang krijgen tot gegevens die worden verzameld of geanalyseerd door Azure-services, inclusief crashdumpbestanden, worden verwerkt door het maken van gebeurtenissen, VM schijf momentopnamen artefacten, waaronder mogelijk per ongeluk gegevens van de klant of persoonlijke gegevens van uw virtuele machines. We voldoen aan de [privacyverklaring voor Microsoft Online Services-voorwaarden en](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), welke status die Microsoft is niet maakt gebruik van gegevens van de klant of een afleiding van gegevens uit het voor reclame of vergelijkbare commerciële doeleinden.

-   **Gegevensgebruik**: Microsoft gebruikt informatie over patronen en bedreigingen die worden gezien tussen meerdere tenants voor het verbeteren van onze mogelijkheden voor voorkoming en detectie; wij doen dit in overeenstemming met de privacyverplichtingen beschreven in onze [Privacyverklaring](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).

### <a name="data-location"></a>Gegevenslocatie

Azure Security Center verzamelt tijdelijke kopieën van uw crashdumpbestanden en analyseert deze op bewijs van pogingen tot misbruik en geslaagde aanvallen. Azure Security Center voert deze analyse uit binnen hetzelfde geografische gebied als de werkruimte en verwijdert de tijdelijke kopieën wanneer de analyse is voltooid. Machine-artefacten worden centraal opgeslagen in dezelfde regio als de virtuele machine.

-   **Uw Opslagaccounts**: een opslagaccount is opgegeven voor elke regio waarin virtuele machines worden uitgevoerd. Hierdoor kunt u gegevens in dezelfde regio opslaan als de virtuele machine waarvan de gegevens worden verzameld.

-   **Azure Security Center-opslag**: informatie over beveiligingswaarschuwingen, waaronder waarschuwingen van partners, aanbevelingen en de status van de beveiliging, worden centraal opgeslagen, momenteel in de Verenigde Staten. Deze informatie omvat mogelijk gerelateerde configuratie-informatie en beveiligingsgebeurtenissen die indien nodig zijn verzameld van uw virtuele machines om u de beveiligingswaarschuwing, aanbeveling of de beveiligingsstatus door te geven.


## <a name="azure-monitor"></a>Azure Monitor

De [OMS beveiliging](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) en controleren van de oplossing kunnen IT-actief bewaken alle bronnen, waarmee de impact van beveiligingsincidenten minimaliseren. OMS beveiligings- en Audit hebben security-domeinen die kunnen worden gebruikt voor het bewaken van de resources. Het beveiligingsdomein snelle toegang biedt tot opties, de volgende domeinen worden voor het bewaken van beveiliging behandeld in meer informatie:

-   evaluatie van schadelijke software
-   Update-evaluatie
-   Identiteit en toegang.

Azure Monitor bevat koppelingen naar informatie over specifieke typen resources. Biedt visualisatie, query routering, waarschuwingen, automatisch schalen en automatisering op gegevens zowel in de Azure-infrastructuur (activiteitenlogboek) en elke afzonderlijke Azure-resource (diagnostische logboeken).

![Azure Monitor](./media/azure-operational-security/azure-operational-security-fig6.png)


Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing up blijft en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen uit het verleden zijn.

Bovendien kunt u bewakingsgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren.

### <a name="azure-activity-log"></a>Azure Activity Log


Het is een logboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Het activiteitenlogboek heette vroeger 'Controlelogboeken' of 'Operationele Logs', omdat het besturingselement vlak gebeurtenissen voor uw abonnementen rapporten.

![Azure Activity Log](./media/azure-operational-security/azure-operational-security-fig7.png)

Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die zijn gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Het activiteitenlogboek omvat geen leesbewerkingen (GET) of -bewerkingen voor resources die gebruikmaken van het klassieke model.

### <a name="azure-diagnostic-logs"></a>Azure diagnostische logboeken

Deze logboeken worden gegenereerd door een resource en leveren van uitgebreide, regelmatig gegevens over de werking van de bron. De inhoud van deze logboeken varieert per resourcetype.

Bijvoorbeeld Windows-gebeurtenislogboeken system zijn één categorie van diagnostische logboeken voor virtuele machines en blob, table en queue logboeken zijn categorieën van diagnostische logboeken voor opslagaccounts.

Diagnostische logboeken afwijken van de [activiteitenlogboek (voorheen bekend als het auditlogboek of operationeel logboek)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Het activiteitenlogboek verschaft inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Diagnostische logboeken bieden inzicht in bewerkingen dat de bron zelf uitgevoerd.

### <a name="metrics"></a>Metrische gegevens

Monitor voor Azure kunt u telemetrie om meer inzicht verkrijgen in de prestaties en de status van uw workloads in Azure gebruiken. De belangrijkste type Azure telemetriegegevens is de metrische gegevens die (ook wel prestatiemeteritems) die door de meeste Azure-resources. Azure biedt verschillende manieren configureren en gebruiken deze [metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) voor bewaking en probleemoplossing. Metrische gegevens zijn een waardevolle bron van Telemetrie en kunt u de volgende taken uitvoeren:

-   **De prestaties bijhouden** van de bron (zoals een VM, website of logic app) door de metrische gegevens over een portal grafiek uitzetten en dat de grafiek aan een dashboard vastmaken.

-   **Blijf op de hoogte van een probleem** die van invloed is op de prestaties van uw resource wanneer een metriek een bepaalde drempelwaarde overschrijden.

-   **Configureer automatische acties**, zoals automatisch schalen van een resource of een runbook wordt gestart wanneer een metriek een bepaalde drempelwaarde overschrijden.

-   **Geavanceerde analyses uitvoeren** of rapportage over trends prestatie- of informatie over het gebruik van de bron.

-   **Archief** de geschiedenis van de prestaties of de status van de bron voor controledoeleinden of compatibiliteit.

### <a name="azure-diagnostics"></a>Azure Diagnostics

De mogelijkheid in Azure waarmee het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing is. U kunt de extensie voor diagnostische gegevens uit diverse verschillende bronnen. Op dit moment ondersteund zijn [Azure Cloud Service-Web- en werkrollen](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/overview) met Microsoft Windows, en [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Andere Azure-services hebben hun eigen afzonderlijke diagnostische gegevens.

## <a name="azure-network-watcher"></a>Azure-netwerk-Watcher

Controle van de netwerkbeveiliging van uw is het essentieel zijn voor netwerk beveiligingsproblemen detecteren en de naleving van uw IT-beveiliging en regelgeving governance model. Overzicht van de beveiligingsgroep, kunt u de geconfigureerde Netwerkbeveiligingsgroep en beveiligingsregels voor verbindingen en de regels voor een effectieve beveiligingsmethode ophalen. Met de lijst met regels die zijn toegepast, kunt u de poorten die zijn geopend en beoordelen van network vulnerability bepalen.

[Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op het netwerkniveau van een in, naar en van Azure. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure. Deze service omvat pakketopname, volgende hop, IP-stroom controleren, groep beveiligingsweergave, NSG stroom Logboeken. Scenario niveau bewaking biedt een complete weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

![Azure-netwerk-Watcher](./media/azure-operational-security/azure-operational-security-fig8.png)

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Controlelogboeken</a>**-bewerkingen die worden uitgevoerd als onderdeel van de configuratie van netwerken worden geregistreerd. Deze logboeken kunnen worden weergegeven in de Azure-portal of opgehaald met behulp van Microsoft-hulpprogramma's zoals Power BI of hulpprogramma's van derden. Controlelogboeken zijn beschikbaar via de portal, PowerShell, CLI en Rest-API. Zie voor meer informatie over controlelogboeken Audit-bewerkingen met Resource Manager. Controlelogboeken zijn beschikbaar voor bewerkingen die op alle netwerkbronnen.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP-stroom controleert of </a>**  -controleert of een pakket wordt toegestaan of geweigerd op basis van stroom informatie 5-tuple pakket parameters (doel-IP, bron-IP, doelpoort, bronpoort en Protocol). Als het pakket is geweigerd door een Netwerkbeveiligingsgroep, wordt de regel en de Netwerkbeveiligingsgroep die het pakket geweigerd geretourneerd.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Volgende hop</a>**  -bepaalt de volgende hop voor pakketten worden gerouteerd in de Azure-netwerk Fabric, zodat u voor het vaststellen van een gebruiker gedefinieerde routes onjuist geconfigureerd.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Groep beveiligingsweergave</a>**  -opgehaald van de effectieve en toegepaste beveiligingsregels voor verbindingen die worden toegepast op een virtuele machine.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">Logboekregistratie NSG Flow</a>**  -stroom in de logboeken voor Netwerkbeveiligingsgroepen kunt u de logboeken die betrekking hebben op het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels voor verbindingen in de groep opnemen. De stroom wordt gedefinieerd door de gegevens van een 5-tuple: bron-IP, doel-IP, bronpoort, doelpoort en -Protocol.

## <a name="azure-storage-analytics"></a>Azure Storage Analytics

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) metrische gegevens die samengevoegde statistieken en capaciteit transactiegegevens over aanvragen met storage-service zijn kan opslaan. Transacties worden gerapporteerd op zowel het API bewerking niveau en op het niveau van de service storage en capaciteit op het niveau van de service storage wordt gerapporteerd. Metrische gegevens kan worden gebruikt om storage-service analyseren, onderzoeken van problemen met aanvragen ten opzichte van de storage-service en verbeteren de prestaties van toepassingen die gebruikmaken van een service.

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) voert logboekregistratie en voorziet in metrische gegevens voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Logboekregistratie van Storage Analytics is beschikbaar voor de [services Blob, wachtrijen en tabellen](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd.

Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met de storage-service. Aanvragen worden geregistreerd op basis van best-effort. Logboekvermeldingen worden alleen gemaakt als er aanvragen voor het service-eindpunt zijn. Voor bijvoorbeeld als een opslagaccount heeft een activiteit in de Blob-eindpunt maar niet in de tabel of een wachtrij-eindpunten alleen registreert wordt met betrekking tot de Blob-service gemaakt.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt het inschakelen in de [Azure-portal](https://portal.azure.com/); voor meer informatie Zie [een opslagaccount in de Azure-portal bewaken](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). U kunt ook Opslaganalyse programmatisch via de REST-API of de clientbibliotheek inschakelen. De bewerking Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service gebruiken.

De samengevoegde gegevens wordt opgeslagen in een bekende blob (voor logboekregistratie) en in een bekende tabellen (voor metrische gegevens) waartoe toegang kunnen worden verkregen met behulp van de Blob-service en de API's van de tabel-service.

Storage Analytics mag maximaal 20 TB van de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw opslagaccount. Alle logboeken worden opgeslagen in [blok-blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in een container met de naam $logs, die automatisch worden gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount.

De volgende acties uitgevoerd door Storage Analytics zijn factureerbare:

-   Aanvragen voor het maken van blobs voor logboekregistratie
-   Aanvragen voor het maken van de tabelentiteiten voor de metrische gegevens.

> [!Note]
> Zie voor meer informatie over facturering en bewaarbeleid voor gegevens [Storage Analytics en facturering](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> Voor optimale prestaties die u wilt beperken het aantal maximaal gebruikte schijven die zijn gekoppeld aan de virtuele machine om te voorkomen dat mogelijk beperking. Als u alle schijven worden niet maximaal worden benut op hetzelfde moment, kan het storage-account een groter aantal schijf te ondersteunen.

> [!Note]
> Zie voor meer informatie over opslagaccountlimieten [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


De volgende soorten geverifieerde en anonieme aanvragen worden geregistreerd.

| Geauthenticeerd  | Anoniem|
| :------------- | :-------------|
| Geslaagde aanvragen | Geslaagde aanvragen |
|Mislukte aanvragen, met inbegrip van de time-outperiode, beperking, netwerk, autorisatie en andere fouten | -Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen |
| -Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen |Time-out fouten voor zowel client als server |
|   Aanvragen voor analytische gegevens |    GET-aanvragen is mislukt met foutcode 304 (niet gewijzigd) |
| Aanvragen door Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) onderwerpen. | Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |
## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD bevat ook een volledige reeks mogelijkheden voor identiteitsbeheer waaronder multi-factor authentication-server, apparaatregistratie, selfservice voor wachtwoordherstel management, self-service groepsbeheer, bevoegd accountmanagement, op rollen gebaseerde toegangsbeheer, gebruik bewaking, uitgebreide controle en beveiligingsbewaking en waarschuwingen.

-   Verbeterde Toepassingsbeveiliging met Azure AD multifactor-verificatie en voorwaardelijke toegang.

-   Controleren van het gebruik en uw bedrijf beschermen tegen geavanceerde bedreigingen met beveiliging rapportage en bewaking.

Azure Active Directory (Azure AD) bevat beveiligings-, activiteits- en controlerapporten voor uw directory. [Het Azure Active Directory auditrapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) klanten helpt bij het identificeren van bevoegde acties die is opgetreden in Azure Active Directory. Bevoegdheden omvatten wijzigingen van de uitbreiding van bevoegdheden (bijvoorbeeld het maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleid) of wijzigingen in de directoryconfiguratie (bijvoorbeeld wijzigingen in de federation-domeininstellingen).

De rapporten bevatten de controlerecord voor de naam van de gebeurtenis, de actor die de actie, de doelresource van invloed op een door de wijziging, en de datum en tijd (in UTC) wordt uitgevoerd. Klanten kunnen ophalen van de lijst van controlegebeurtenissen voor Azure Active Directory via de [Azure-portal](https://portal.azure.com/), zoals beschreven in [uw logboeken Audit](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Hierna volgt een lijst met de beschikbare rapporten:

| Beveiligingsrapporten  | Activiteitsrapporten| Controlerapporten |
| :------------- | :-------------| :-------------|
|Aanmeldingen van onbekende bronnen | Toepassingsgebruik: samenvatting | Directorycontrolerapport |
|Aanmeldingen na meerdere mislukte pogingen | Toepassingsgebruik: gedetailleerd |   |
|Aanmeldingen vanuit meerdere locaties | Toepassingsdashboard |  |
|Aanmeldingen van IP-adressen met verdachte activiteit |Fouten bij het inrichten van een account |  |
|Onregelmatige aanmeldingsactiviteiten |Afzonderlijke gebruikersapparaten |  |
|Aanmeldingen vanaf mogelijk geïnfecteerde apparaten |Afzonderlijke gebruikersactiviteiten |   |
|Gebruikers met afwijkende aanmeldingsactiviteiten |Rapport met groepsactiviteiten |   |
| |Rapport voor de registratie van opnieuw ingestelde wachtwoorden |   |
| |Activiteit voor wachtwoord opnieuw instellen |   | |



De gegevens van deze rapporten kunnen nuttig zijn voor uw toepassingen, zoals de SIEM-systemen, controle en hulpprogramma's voor bedrijfsinformatie. De Azure AD-rapportage [API's](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) programmatisch toegang biedt tot de gegevens via een set op basis van REST-API's. U kunt deze API aanroepen vanuit verschillende programmeertalen en hulpprogramma's.

Gebeurtenissen in de Azure AD-controlerapport worden bewaard gedurende 180 dagen.

> [!Note]
> Zie voor meer informatie over de bewaarperiode op rapporten [bewaarbeleid voor Azure Active Directory rapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Voor klanten die de bij het opslaan van hun [controlegebeurtenissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) voor langere bewaartermijn de rapportage-API kan worden gebruikt voor het ophalen van controlegebeurtenissen regelmatig in een afzonderlijke gegevensarchief.

## <a name="summary"></a>Samenvatting

Dit artikel samenvattingen van de bescherming van uw privacy en beveiliging van uw gegevens tijdens de levering van software en services die u helpen beheren de IT-infrastructuur van uw organisatie. Microsoft herkent dat wanneer ze hun gegevens naar anderen belasten, vertrouwensrelatie strengere beveiliging vereist. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service. Beveiliging en bescherming van gegevens is een topprioriteit bij Microsoft.

Dit artikel wordt uitgelegd

-   Hoe gegevens wordt verzameld, verwerkt en beveiligd in de Operations Management Suite (OMS).

-   Analyseer snel gebeurtenissen in meerdere gegevensbronnen. Identificeer beveiligingsrisico's en wees op de hoogte van de omvang en impact van bedreigingen en aanvallen om schade na inbreuk op de beveiliging te voorkomen.

-   Identificeer aanvalspatronen door uitgaand, schadelijk IP-verkeer en schadelijke bedreigingstypen te visualiseren. De beveiligingsstatus van uw hele omgeving ongeacht platform begrijpen.

-   Alle logboekbestanden en de gebeurtenis vereiste gegevens voor een controle beveiligings- of naleving vastleggen. De tijd en bronnen die nodig zijn om op te geven van een beveiligingscontrole met een volledig, doorzoekbaar en exporteerbaar logboek en gegevensset voor gebeurtenis slash.

<ul>
<li>Verzamelt gebeurtenissen die betrekking hebben op beveiliging, controle en inbreuk op de analyse wilt gaten Houd uw activa:</li>
<ul>
<li>Beveiligingspostuur</li>
<li>Opmerkelijke probleem</li>
<li>Samenvattingen bedreigingen</li>
</ul>
</ul>

## <a name="next-steps"></a>Volgende stappen

- [Ontwerp- en operationele beveiliging](https://www.microsoft.com/trustcenter/security/designopsecurity)

Microsoft ontwerpen van de services en software beveiligd zijn om ervoor te zorgen dat de cloudinfrastructuur robuuste en beveiligd zijn tegen aanvallen is.

- [Operations Management Suite | Beveiliging en naleving](https://www.microsoft.com/cloud-platform/security-and-compliance)

Gebruik Microsoft beveiligingsgegevens en -analyse intelligente en effectieve bedreigingsdetectie uitvoeren.

- [Azure Security Center plannen en bewerkingen](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) een reeks stappen en taken die u volgen kunt om het optimaliseren van uw gebruik van het Beveiligingscentrum op basis van de beveiligingsvereisten en het cloudbeheermodel van uw organisatie.

