---
title: Operationele beveiliging in Azure | Microsoft Docs
description: Meer informatie over Microsoft Azure Log Analytics, de services en hoe het werkt.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: c7d8ab8beced055d10cebcecf36d957b155c63b7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256218"
---
# <a name="azure-operational-security"></a>Azure operationele beveiliging
## <a name="introduction"></a>Inleiding

### <a name="overview"></a>Overzicht
We weten dat de beveiliging is taak in de cloud en hoe belangrijk is dat u nauwkeurige en tijdig informatie over Azure-beveiliging vinden. Een van de beste redenen om Azure te gebruiken voor uw toepassingen en services is om te profiteren van de grote verscheidenheid aan hulpprogramma's voor beveiliging en mogelijkheden die beschikbaar. Deze hulpprogramma's en mogelijkheden helpt u bij het mogelijk te maken van veilige oplossingen op de beveiligde Azure-platform. Windows Azure moet vertrouwelijkheid, integriteit en beschikbaarheid van gegevens van de klant, opgeven tijdens het inschakelen van ook transparante aansprakelijkheid.

Biedt een uitgebreide zodat klanten beter begrip van de matrix van beveiligingsmechanismen geïmplementeerd in Microsoft Azure van zowel de klant en Microsoft operationele perspectieven, deze whitepaper, 'Azure operationele beveiliging', die is geschreven. Bekijk de operationele beveiliging die beschikbaar zijn in Windows Azure.

### <a name="azure-platform"></a>Azure-Platform
Azure is een openbare cloud service-platform die ondersteuning biedt voor een breed scala aan besturingssystemen, programmeertalen programmeertalen, frameworks, tools, databases en apparaten. Het kunt Linux-containers uitvoeren met Docker-integratie; Bouw apps met JavaScript, Python, .NET, PHP, Java en Node.js; Bouw back-ends voor iOS, Android en Windows apparaten. Azure cloudservice ondersteunt dezelfde technologieën waar miljoenen ontwikkelaars en IT-professionals die al afhankelijk zijn van en vertrouwen.

Wanneer u baseren of IT-activa te migreren, een openbare cloud serviceprovider u, vertrouwen al op de beveiligingmogelijkheden die aan uw toepassingen en gegevens beschermen met de services en de besturingselementen deze opgeven voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

De opslaginfrastructuur van Azure van de faciliteit is zo ontworpen toepassingen miljoenen klanten tegelijkertijd kunnen hosten en biedt een betrouwbare basis wordt geboden waarop bedrijven kunnen voldoen aan de beveiligingsvereisten. Bovendien biedt Azure u een breed scala aan configureerbare beveiligingsopties en de mogelijkheid om deze te beheren zodat u de beveiliging om te voldoen aan de unieke vereisten van implementaties van uw organisatie kunt aanpassen. Dit document wordt helpt u begrijpen hoe Azure-beveiliging mogelijkheden kunt u aan deze vereisten voldoen.

### <a name="abstract"></a>Samenvatting
Azure operationele beveiliging verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere items in Microsoft Azure. Operationele beveiliging in Azure is gebouwd op een framework waarin de kennis opgedaan uit verschillende mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Center en een diep besef van het landschap van cyberveiligheidsbedreigingen.

Dit technische document geeft een overzicht van de Microsoft-benadering tot operationele beveiliging van Azure binnen de Microsoft Azure-cloud-platform en omvat de volgende services:
1.  [Azure Monitor](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Azure Network watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-azure-log-analytics"></a>Microsoft Azure-logboekanalyse

Microsoft Azure Log Analytics is de IT-beheeroplossing voor de hybride cloud. Alleen wordt gebruikt of om uit te breiden van uw bestaande System Center-implementatie, Log Analytics, zodat u de maximale flexibiliteit en controle voor cloud-gebaseerd beheer van uw infrastructuur.

![Log Analytics](./media/azure-operational-security/azure-operational-security-fig1.png)

Met Log Analytics, kunt u een instantie in elke cloud, met inbegrip van on-premises, Azure, AWS, Windows Server, Linux, VMware en OpenStack, tegen lagere kosten dan concurrerende, door oplossingen te beheren. Log Analytics is gebouwd voor de cloudgeoriënteerde wereld, biedt een nieuwe benadering voor het beheren van uw bedrijf dat is de snelste en meest rendabele manier om te voldoen aan de nieuwe zakelijke uitdagingen en geschikt voor de nieuwe werkbelastingen, toepassingen en cloudomgevingen.

### <a name="log-analytics-services"></a>Log Analytics-services

De kernfunctionaliteit van Log Analytics wordt geboden door een set met services die worden uitgevoerd in Azure. Elke service biedt een specifieke beheerfunctie. U kunt services combineren om verschillende beheerscenario's te bewerkstelligen.

| Service  | Beschrijving|
| :------------- | :-------------|
| Log Analytics | Bewaak en analyseer de beschikbaarheid en prestaties van verschillende resources, met inbegrip van fysieke en virtuele machines. |
|Automation | Automatiseer handmatige processen en dwing configuraties af voor fysieke en virtuele machines. |
| Backup | Back-up en herstellen van kritieke gegevens. |
| Site Recovery | Bied hoge beschikbaarheid voor kritieke toepassingen. |

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics) bewakingsservices biedt door het verzamelen van gegevens van beheerde resources in een centrale opslagplaats. Deze gegevens kunnen gebeurtenissen, prestatiegegevens en aangepaste gegevens omvatten die via de API worden geleverd. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export.


Deze methode kunt u gegevens uit verschillende bronnen consolideren, zodat u kunt combineren gegevens uit uw Azure-services met uw bestaande on-premises omgeving. De methode maakt ook een duidelijk onderscheid tussen het verzamelen van gegevens en het bewerken hiervan. Zo zijn alle bewerkingen beschikbaar voor alle soorten gegevens.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

De service Log Analytics beheert uw cloud-gebaseerde gegevens veilig met behulp van de volgende methoden:
-   scheiding van gegevens
-   bewaren van gegevens
-   fysieke beveiliging
-   incidentbeheer
-   naleving
-   standaardcertificeringen van beveiliging

### <a name="azure-backup"></a>Azure Backup

[Azure Backup](https://azure.microsoft.com/documentation/services/backup) biedt gegevens back-up en herstellen van services en maakt deel uit van de Log Analytics-suite met producten en services.
Het beschermt uw toepassingsgegevens en bewaart deze jarenlang, zonder dat u grote investeringen hoeft te doen en met een minimum aan operationele kosten. Back-up kan gegevens van fysieke en virtuele Windows-servers en werkbelastingen van toepassingen zoals SQL Server en SharePoint. Kan ook worden gebruikt door [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) beveiligde om gegevens te repliceren naar Azure voor redundantie en opslag op lange termijn.


Beveiligde gegevens in Azure Backup worden opgeslagen in een back-upkluis in een bepaalde geografische regio. De gegevens binnen dezelfde regio gerepliceerd en, afhankelijk van het type kluis, kan ook worden gerepliceerd naar een andere regio voor meer flexibiliteit.

### <a name="management-solutions"></a>Beheeroplossingen
[Log Analytics](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) is van Microsoft cloud-gebaseerde IT-beheeroplossing waarmee u beheren kunt en beveiligen van uw on-premises en cloudinfrastructuur.


[Beheeroplossingen](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) zijn voorverpakte sets met logics die een bepaald beheerscenario met behulp van een of meer Log Analytics-services te implementeren. Er zijn verschillende oplossingen beschikbaar van Microsoft en partners die u kunt eenvoudig toevoegen aan uw Azure-abonnement te verhogen van de waarde van uw investering in Log Analytics. Als partner kunt u uw eigen oplossingen voor het ondersteunen van uw toepassingen en services en ze aanbieden aan gebruikers via de Azure Marketplace of snel starten-sjablonen maken.


![Beheeroplossingen](./media/azure-operational-security/azure-operational-security-fig4.png)

Een goed voorbeeld van een oplossing die gebruikmaakt van meerdere services om extra functionaliteit te bieden is de [oplossing Update Management](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Deze oplossing maakt gebruik van de [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) -agent voor Windows en Linux om gegevens te verzamelen over vereiste updates op elke agent. Het schrijft deze gegevens naar de opslagplaats van Log Analytics waar u ze kunt analyseren met een inbegrepen dashboard.

Wanneer u een implementatie van runbooks in maakt [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) worden gebruikt voor het installeren van vereiste updates. U beheert dit hele proces in de portal en hoeft zich geen zorgen te maken over de onderliggende details.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center helpt te beschermen van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen. In de service, kunt u definiëren niet alleen op basis van uw Azure-abonnementen, maar ook het beleid op basis van [resourcegroepen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), zodat u gedetailleerdere.

### <a name="security-policies-and-recommendations"></a>Beveiligingsbeleid en aanbevelingen

Een beveiligingsbeleid bepaalt welke set besturingselementen wordt aanbevolen voor resources binnen het opgegeven abonnement of de opgegeven resourcegroep.

In Security Center definieert u de beleidsregels op grond van de beveiligingsvereisten van uw bedrijf en het type toepassingen of de gevoeligheid van de gegevens.

![Beveiligingsbeleid en aanbevelingen](./media/azure-operational-security/azure-operational-security-fig5.png)


Beleidsregels die zijn ingeschakeld op het abonnementsniveau automatisch doorgegeven naar alle resourcegroepen binnen het abonnement, zoals weergegeven in het diagram aan de rechterkant:


### <a name="data-collection"></a>Gegevensverzameling

Met Security Center worden gegevens van uw virtuele machines (VM's) verzameld om de beveiligingsstatus van de VM's te beoordelen, aanbevelingen voor beveiliging te geven en u te waarschuwen bij bedreigingen. Wanneer uw eerste toegang Security Center, het verzamelen van gegevens is ingeschakeld op alle virtuele machines in uw abonnement. Gegevensverzameling wordt aanbevolen, maar u kunt gegevensverzameling indien gewenst ook uitschakelen in het Security Center-beleid.

### <a name="data-sources"></a>Gegevensbronnen

- Azure Security Center analyseert gegevens uit de volgende bronnen om inzicht in uw beveiligingsstatus te geven, beveiligingsproblemen te identificeren, oplossingen aan te raden en actieve bedreigingen te detecteren:

-   Azure Services: gebruikt informatie over de configuratie van de Azure-services die u hebt geïmplementeerd door te communiceren met de resourceprovider van die service.

- Netwerkverkeer: gebruikt steekproefgewijs netwerkverkeermetagegevens uit de infrastructuur van Microsoft, zoals bron-/doel-IP/poort, pakketgrootte en netwerkprotocol.

-   Oplossingen van partners: gebruikt beveiligingswaarschuwingen van geïntegreerde partneroplossingen, zoals firewalls en antimalwareoplossingen.

-   Uw virtuele machines: gebruikt configuratie-informatie en informatie over beveiligingsgebeurtenissen, zoals Windows-gebeurtenis- en auditlogboeken, IIS-logboeken, syslog-berichten en crashdumpbestanden van uw virtuele machines.

### <a name="data-protection"></a>Gegevensbeveiliging

Om klanten te helpen bedreigingen te voorkomen, te detecteren en erop te reageren, verzamelt en verwerkt Azure Security Center gegevens over beveiliging, zoals configuratie-informatie, metagegevens, gebeurtenislogboeken, crashdumpbestanden en nog veel meer. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service.

-   **Scheiding van gegevens**: gegevens worden op een logische manier apart van elkaar gehouden, in elk onderdeel van de service. Alle gegevens worden gemarkeerd per organisatie. Deze markering blijft aanwezig gedurende de levenscyclus van de gegevens en deze wordt afgedwongen op elke laag van de service.

-   **Toegang tot gegevens**: om aanbevelingen voor beveiliging bieden en mogelijke bedreigingen te onderzoeken, Microsoft-personeel kunnen toegang krijgen tot gegevens die worden verzameld of geanalyseerd door Azure-services, met inbegrip van crashdumpbestanden, procesgebeurtenissen, VM-schijf verwerken momentopnamen en artefacten, waaronder per ongeluk klantgegevens of persoonlijke gegevens van uw virtuele machines. We voldoen aan de [privacyverklaring voor Microsoft Online Services-voorwaarden en](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), waarin staat dat Microsoft geen maakt gebruik van klantgegevens of afleidt voor reclame- of vergelijkbare commerciële doeleinden.

-   **Gegevensgebruik**: Microsoft gebruikt informatie over patronen en bedreigingen die worden gezien tussen meerdere tenants voor het verbeteren van onze mogelijkheden voor voorkoming en detectie; wij doen dit in overeenstemming met de privacyverplichtingen beschreven in onze [Privacyverklaring](https://www.microsoft.com/en-us/privacystatement/OnlineServices/).

### <a name="data-location"></a>Gegevenslocatie

Azure Security Center verzamelt tijdelijke kopieën van uw crashdumpbestanden en analyseert deze op bewijs van pogingen tot misbruik en geslaagde aanvallen. Azure Security Center voert deze analyse uit binnen hetzelfde geografische gebied als de werkruimte en verwijdert de tijdelijke kopieën wanneer de analyse is voltooid. Machine-artefacten worden centraal opgeslagen in dezelfde regio als de virtuele machine.

-   **Uw Opslagaccounts**: een storage-account is opgegeven voor elke regio waarin de virtuele machines worden uitgevoerd. Hierdoor kunt u gegevens in dezelfde regio opslaan als de virtuele machine waarvan de gegevens worden verzameld.

-   **Azure Security Center-opslag**: informatie over beveiligingswaarschuwingen, waaronder waarschuwingen van partners, aanbevelingen en de status van de beveiliging, worden centraal opgeslagen, momenteel in de Verenigde Staten. Deze informatie omvat mogelijk gerelateerde configuratie-informatie en beveiligingsgebeurtenissen die indien nodig zijn verzameld van uw virtuele machines om u de beveiligingswaarschuwing, aanbeveling of de beveiligingsstatus door te geven.


## <a name="azure-monitor"></a>Azure Monitor

De [Log Analytics Security](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) en controle-oplossing kunnen IT-actief Controleer alle resources, waarmee het effect van beveiligingsincidenten minimaliseren. Log Analytics-beveiliging en controle hebt beveiligingsdomeinen die kunnen worden gebruikt voor het bewaken van resources. Het beveiligingsdomein biedt snel toegang tot de opties, voor de beveiligingsbewaking de volgende domeinen worden behandeld in meer informatie:

-   Malware-evaluatie
-   Update-evaluatie
-   Identiteits- en toegangsbeheer.

Azure Monitor biedt verwijzingen naar informatie over specifieke typen resources. Het biedt visualisatie, query, routering, waarschuwingen, automatisch schalen en automatisering op gegevens zowel in de Azure-infrastructuur (activiteitenlogboek) en elke afzonderlijke Azure-resource (diagnostische logboeken).

![Azure Monitor](./media/azure-operational-security/azure-operational-security-fig6.png)


Cloud-Apps zijn complexe met veel bewegende onderdelen bevatten. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing actief en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen van het verleden zijn.

Bovendien kunt u bewakingsgegevens diep om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen te verbeteren van de prestaties van de toepassing of onderhoud, of Automatiseer acties die anders handmatig worden opgelost moeten zouden.

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek


Het is een logboek dat u inzicht biedt in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Het activiteitenlogboek was voorheen bekend als 'Audit Logs' of 'Operationele Logs', omdat deze controlelaag gebeurtenissen voor uw abonnementen rapporten.

![Azure-activiteitenlogboek](./media/azure-operational-security/azure-operational-security-fig7.png)

Met het activiteitenlogboek, kunt u bepalen de ' wat, wie, en wanneer ' voor (PUT, POST, DELETE schrijfbewerkingen) die wordt gemaakt op de resources in uw abonnement. U kunt ook de status van de bewerking en andere relevante eigenschappen begrijpen. Het activiteitenlogboek bevat geen lees (GET)-bewerkingen of bewerkingen voor resources die gebruikmaken van het klassieke model.

### <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure

Deze logboeken worden gegenereerd door een resource en biedt uitgebreide, regelmatig gegevens over de werking van die resource. Resourcetype is afhankelijk van de inhoud van deze logboeken.

Bijvoorbeeld: Windows-gebeurtenislogboeken system zijn één categorie van diagnostische logboeken voor virtuele machines en blob, table en wachtrijlogboeken zijn categorieën van diagnostische logboeken voor opslagaccounts.

Diagnoselogboeken verschillen van de [activiteitenlogboek (voorheen bekend als het auditlogboek of operationeel logboek)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Het activiteitenlogboek biedt inzicht in de bewerkingen die zijn uitgevoerd op resources in uw abonnement. Diagnoselogboeken bieden inzicht in bewerkingen die door de resources zelf zijn uitgevoerd.

### <a name="metrics"></a>Metrische gegevens

Azure Monitor kunt u telemetrie voor inzicht in de prestaties en status van uw werklasten op Azure gebruiken. De belangrijkste type Azure telemetrische gegevens is de metrische gegevens die (ook wel prestatiemeteritems) gegenereerd door de meeste Azure-resources. Azure Monitor biedt verschillende manieren om te configureren en gebruiken deze [metrische gegevens](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) voor controle en probleemoplossing. Metrische gegevens vormen een waardevolle bron van Telemetrie en kunt u de volgende taken uitvoeren:

-   **De prestaties bijhouden** van uw resource (zoals een virtuele machine, de website of de logische app) door de metrische gegevens over een portal grafiek getekend en grafiek aan een dashboard vastmaken.

-   **Blijf op de hoogte van een probleem** die van invloed is op de prestaties van uw resource als een metriek een bepaalde drempelwaarde overschrijdt.

-   **Configureren van automatische acties**, zoals automatisch schalen van een resource of een runbook wordt geactiveerd wanneer een metriek een bepaalde drempelwaarde overschrijdt.

-   **Geavanceerde analyses uitvoeren** of rapporten op trends die grote prestaties of het gebruik van uw resource.

-   **Archief** de geschiedenis van prestaties of de status van uw resource voor naleving of controledoeleinden.

### <a name="azure-diagnostics"></a>Azure Diagnostics

Het is de mogelijkheid in Azure waarmee u het verzamelen van diagnostische gegevens op een geïmplementeerde toepassing. U kunt de extensie voor diagnostische gegevens gebruiken uit diverse verschillende bronnen. Op dit moment ondersteund zijn [Azure Cloud Service-Web- en werkrollen](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/overview) met Microsoft Windows, en [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Andere Azure-services hebben hun eigen afzonderlijke diagnostische gegevens.

## <a name="azure-network-watcher"></a>Azure Network Watcher

Controle van de netwerkbeveiliging van uw is het essentieel is voor het netwerk beveiligingsproblemen detecteren en ervoor te zorgen dat voldoet aan uw IT-beveiliging- en regelgeving governance-model. Met de weergave van de beveiligingsgroep, kunt u de geconfigureerde Netwerkbeveiligingsgroep en beveiligingsregels voor verbindingen en de effectieve beveiligingsregels ophalen. Met de lijst met regels die worden toegepast, kunt u de poorten die zijn geopend en beoordeling van beveiligingslek in netwerk bepalen.

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) is een regionale service waarmee u kunt bewaken en diagnosticeren op het niveau van een netwerk in, naar en van Azure. Diagnose en visualisatie hulpprogramma's die beschikbaar zijn met Network Watcher kunnen u begrijpen, diagnosticeren en inzicht verkrijgen in uw netwerk in Azure. Deze service omvat pakket vastleggen, volgende hop, IP-stroom controleren, weergave van de beveiligingsgroep, NSG-stroomlogboeken. Scenario niveau bewaking biedt een end-to-weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

![Azure Network Watcher](./media/azure-operational-security/azure-operational-security-fig8.png)

Netwerk-Watcher heeft momenteel de volgende mogelijkheden:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">Auditlogboeken</a>**-bewerkingen die worden uitgevoerd als onderdeel van de configuratie van netwerken worden geregistreerd. Deze logboeken kunnen worden weergegeven in de Azure-portal of opgehaald met behulp van Microsoft-hulpprogramma's zoals Power BI of hulpprogramma's van derden. Auditlogboeken zijn beschikbaar via de portal, PowerShell, CLI en Rest-API. Zie voor meer informatie over de logboeken voor controle, bewerkingen controleren met Resource Manager. Auditlogboeken zijn beschikbaar voor bewerkingen die op alle netwerkbronnen.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP-stroom wordt gecontroleerd </a>**  -controleert of een pakket wordt toegestaan of geweigerd op basis van flow informatie 5-tuple pakket parameters (doel-IP, bron-IP, doelpoort, bronpoort en Protocol). Als het pakket is geweigerd door een Netwerkbeveiligingsgroep, wordt de regel en de Netwerkbeveiligingsgroep die het pakket geweigerd geretourneerd.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Volgende hop</a>**  -bepaalt van de volgende hop voor pakketten worden gerouteerd in de netwerkinfrastructuur in Azure, zodat u kunt voor het vaststellen van een gebruiker gedefinieerde routes onjuist geconfigureerd.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Weergave van de beveiligingsgroep</a>**  -de kracht en toegepaste beveiligingsregels die worden toegepast op een virtuele machine opgehaald.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">Stroomlogboeken logboekregistratie</a>**  -stroomlogboeken voor Netwerkbeveiligingsgroepen kunnen u voor het vastleggen van logboeken met betrekking tot het verkeer dat wordt toegestaan of geweigerd door de beveiligingsregels in de groep. De stroom wordt gedefinieerd door een 5-tuple-informatie, bron-IP, doel-IP, doelpoort, bronpoort en Protocol.

## <a name="azure-storage-analytics"></a>Azure Storage Analytics

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) kunt metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens gegevens over aanvragen naar een storage-service. Transacties worden gerapporteerd op zowel de API bewerking-niveau en op het niveau van de service opslag en capaciteit wordt gerapporteerd op het niveau van de storage-service. Metrische gegevens kan worden gebruikt storage service-gebruik analyseren, problemen diagnosticeren met aanvragen voor de storage-service en voor het verbeteren van de prestaties van toepassingen die gebruikmaken van een service.

[Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) logboekregistratie uitvoert en metrische gegevens biedt voor een opslagaccount. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount. Logboekregistratie van opslag Analytics is beschikbaar voor de [services Blob, Queue en Table](https://docs.microsoft.com/azure/storage/storage-introduction). Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd.

Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en om problemen met een opslagservice te diagnosticeren. Aanvragen worden geregistreerd op basis van best-effort. Logboekvermeldingen worden alleen als er aanvragen voor het service-eindpunt gemaakt. Voor voorbeeld als een storage-account heeft een activiteit in de Blob-eindpunt, maar niet in de tabel of wachtrij-eindpunten, alleen logboeken wordt met betrekking tot de Blob-service gemaakt.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt deze inschakelen in de [Azure-portal](https://portal.azure.com/); voor meer informatie, Zie [een opslagaccount in Azure portal controleren](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de bewerking van de Service-eigenschappen instellen voor het inschakelen van Storage Analytics afzonderlijk voor elke service.

De samengevoegde gegevens worden opgeslagen in een bekende blob (voor logboekregistratie) en in bekende tabellen (voor metrische gegevens), die kunnen worden geopend met behulp van de Blob-service en service-tabel API's.

Storage Analytics heeft een limiet van 20 TB voor de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw storage-account. Alle logboeken worden opgeslagen in [blok-blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in een container met de naam $logs, die automatisch worden gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount.

De volgende acties uitgevoerd door Storage Analytics zijn factureerbaar:

-   Aanvragen voor het maken van blobs voor logboekregistratie
-   Aanvragen voor de tabelentiteiten voor metrische gegevens maken.

> [!Note]
> Zie voor meer informatie over facturering en beleidsregels voor Gegevensretentie [Storage Analytics en facturering](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> Voor optimale prestaties die u wilt beperken van het aantal intensief gebruikte schijven die zijn gekoppeld aan de virtuele machine om te voorkomen dat mogelijk beperken. Als alle schijven zijn niet maximaal worden gebruikt op hetzelfde moment, kan het opslagaccount een groter aantal schijf te ondersteunen.

> [!Note]
> Zie voor meer informatie over opslagaccountlimieten [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


De volgende typen geverifieerde en anonieme aanvragen worden geregistreerd.

| Geverifieerd  | Anoniem|
| :------------- | :-------------|
| Geslaagde aanvragen | Geslaagde aanvragen |
|Mislukte aanvragen, met inbegrip van time-out, beperking, netwerk, autorisatie en andere fouten | Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen |
| Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen |Time-out van fouten voor zowel client als server |
|   Aanvragen voor het analytics-gegevens |    GET-aanvragen is mislukt met foutcode 304 (niet gewijzigd) |
| Aanvragen van Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) onderwerpen. | Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |
## <a name="azure-active-directory"></a>Azure Active Directory

Azure AD bevat ook een volledige suite met mogelijkheden voor identiteitsbeheer zoals meervoudige verificatie, device Registration service, eigen wachtwoordbeheer, selfservice groepsbeheer, bevoegd accountmanagement, op basis van de rol controle, bewaking van toepassingsgebruik, uitgebreide controle, en beveiligingsbewaking en waarschuwingen.

-   Verbeter de beveiliging van toepassingen met Azure AD-meervoudige verificatie en voorwaardelijke toegang.

-   Gebruik van toepassingen bewaken en uw bedrijf beschermen tegen geavanceerde bedreigingen met beveiliging rapportage- en bewakingsdoeleinden.

Azure Active Directory (Azure AD) bevat beveiligings-, activiteits- en controlerapporten voor uw directory. [De Azure Active Directory Audit Report](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) helpt klanten bij het identificeren van bevoegde acties die zijn opgetreden in hun Azure Active Directory. Bevoegde acties zijn wijzigingen van de uitbreiding van bevoegdheden (bijvoorbeeld rol maken of opnieuw instellen van wachtwoorden), veranderende beleidsconfiguraties (bijvoorbeeld wachtwoordbeleid) of wijzigingen in de directoryconfiguratie (bijvoorbeeld: wijzigingen in domein federatie-instellingen).

De rapporten bevatten de controlerecord voor de naam van de gebeurtenis, de actor die de actie, de doelresource beïnvloed door de wijziging en de datum en tijd (in UTC) heeft uitgevoerd. Klanten kunnen de lijst met controlegebeurtenissen ophalen voor hun Azure Active Directory via de [Azure-portal](https://portal.azure.com/), zoals beschreven in [weergeven van de logboeken controleren](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Hierna volgt een lijst met de beschikbare rapporten:

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



De gegevens van deze rapporten kunnen nuttig zijn voor uw toepassingen, zoals SIEM-systemen, audit en hulpprogramma's voor business intelligence. De Azure AD-rapportage [API's](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) bieden programmatisch toegang tot de gegevens via een set op REST gebaseerde API's. U kunt deze API's aanroepen vanuit verschillende programmeertalen en hulpprogramma's.

Gebeurtenissen in de auditlogboeken van Azure AD-rapport worden gedurende 180 dagen bewaard.

> [!Note]
> Zie voor meer informatie over het bewaren van op rapporten [bewaarbeleid voor Azure Active Directory rapport](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Voor klanten die interesse hebben bij het opslaan van hun [controlegebeurtenissen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) voor langere bewaarperioden, de rapportage-API kan worden gebruikt voor het ophalen van gebeurtenissen voor beveiligingscontrole regelmatig in een afzonderlijk gegevensarchief.

## <a name="summary"></a>Samenvatting

In dit artikel samenvattingen van de bescherming van uw privacy en beveiliging van uw gegevens, terwijl de levering van software en services die u helpen bij beheren de IT-infrastructuur van uw organisatie. Microsoft herkent wanneer ze hun gegevens naar anderen entrust, deze vertrouwensrelatie rigoureuze beveiligingscontrole vereist. Microsoft voldoet aan strikte nalevings- en beveiligingsrichtlijnen - van het schrijven van code tot de uitvoering van een service. Beveiligen en beschermen van gegevens is een topprioriteit bij Microsoft.

Dit artikel wordt uitgelegd

-   Hoe data wordt verzameld, verwerkt en beveiligd in de Log Analytics-suite.

-   Analyseer snel gebeurtenissen in meerdere gegevensbronnen. Identificeer beveiligingsrisico's en wees op de hoogte van de omvang en impact van bedreigingen en aanvallen om schade na inbreuk op de beveiliging te voorkomen.

-   Identificeer aanvalspatronen door uitgaand, schadelijk IP-verkeer en schadelijke bedreigingstypen te visualiseren. Begrijp het beveiligingspostuur van uw hele omgeving, ongeacht het platform.

-   Alle logboek- en gebeurtenisgegevens gegevens vereist zijn voor een beveiligings- of -audit vastleggen. Schuine de tijd en resources die nodig zijn om op te geven van een beveiligingscontrole met een volledige, doorzoekbare en exporteerbare logboek- en gebeurtenisgegevensset.

<ul>
<li>Verzamel beveiligingsgerelateerde gebeurtenissen, controle en analyse van inbreuk op uw assets de gaten houden:</li>
<ul>
<li>Beveiligingsstatus</li>
<li>Probleem dat aandacht vereist</li>
<li>Samenvattingen van bedreigingen</li>
</ul>
</ul>

## <a name="next-steps"></a>Volgende stappen

- [Ontwerp en operationele beveiliging](https://www.microsoft.com/trustcenter/security/designopsecurity)

De services en software van Microsoft worden wordt met beveiliging in gedachten om ervoor te zorgen dat de cloudinfrastructuur flexibel en beveiligd tegen aanvallen is.

- [Log Analytics | Beveiliging en naleving](https://www.microsoft.com/cloud-platform/security-and-compliance)

Gebruik Microsoft beveiligingsgegevens en -analyse uit te voeren van intelligente en effectieve bedreigingsdetectie.

- [Azure Security Center planning en bewerkingen](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) een reeks stappen en taken die u volgen kunt om uw gebruik van Security Center op basis van de beveiligingsvereisten en het cloudbeheermodel van uw organisatie te optimaliseren.

