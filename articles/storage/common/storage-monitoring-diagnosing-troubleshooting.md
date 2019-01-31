---
title: Bewaken, diagnosticeren en oplossen van Azure Storage | Microsoft Docs
description: Gebruik functies zoals opslaganalyse, logboekregistratie aan clientzijde en andere hulpprogramma's van derden te identificeren, onderzoeken en oplossen van problemen met betrekking tot Azure Storage.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 05/11/2017
ms.author: fhryo-msft
ms.subservice: common
ms.openlocfilehash: 6b40741545ff286bc1b8e696d28c61b197605247
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55474000"
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage bewaken, problemen opsporen en oplossen
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Overzicht
Vaststellen en oplossen van problemen in een gedistribueerde toepassing die wordt gehost in een cloudomgeving kunnen complexer dan in traditionele omgevingen zijn. Toepassingen kunnen worden geïmplementeerd in een PaaS of IaaS-infrastructuur, on-premises, op een mobiel apparaat of in een combinatie van deze omgevingen. Normaal gesproken netwerkverkeer van uw toepassing kan passeren openbare en particuliere netwerken en toepassingen kan gebruikmaken van meerdere opslagtechnologieën, zoals Microsoft Azure Storage-tabellen, Blobs, wachtrijen of bestanden naast andere gegevens worden opgeslagen die als relationele en documentdatabases.

U moet deze proactief controleren en te begrijpen hoe u problemen vaststellen en oplossen van alle aspecten van deze en hun afhankelijke technologieën voor het beheren van deze toepassingen is. Als een gebruiker van Azure Storage-services, moet u continu bewaken van de Storage-services die uw toepassing wordt gebruikt voor eventuele onverwachte wijzigingen in gedrag (zoals langzamer dan gewoonlijk reactietijden) en logboekregistratie gebruiken om meer gedetailleerde gegevens te verzamelen en analyseren van een probleem in diepte. De diagnostische gegevens die u verkrijgen van controle en logboekregistratie helpt u om te bepalen van de hoofdoorzaak van het probleem dat uw toepassing aangetroffen. U kunt vervolgens het probleem oplossen met en bepaal de juiste stappen die u uitvoeren kunt worden opgelost. Azure Storage is een kern-Azure-service en vormt een belangrijk onderdeel van de meeste oplossingen die klanten in de Azure-infrastructuur implementeert. Azure Storage omvat mogelijkheden om te controleren, vaststellen en oplossen van opslagproblemen in uw cloud-gebaseerde toepassingen vereenvoudigen.

> [!NOTE]
> Azure Files biedt geen ondersteuning voor logboekregistratie op dit moment.
> 

Zie voor een praktische handleiding voor het oplossen van problemen in Azure Storage-toepassingen end-to-end, [End-to-End problemen oplossen met metrische gegevens van Azure Storage en logboekregistratie, AzCopy en Message Analyzer](../storage-e2e-troubleshooting.md).

* [Inleiding]
  * [Hoe deze handleiding is georganiseerd]
* [Bewaking van uw storage-service]
  * [Servicestatus controleren]
  * [Bewaking van capaciteit]
  * [Bewaken van de beschikbaarheid]
  * [Prestaties bewaken]
* [Diagnose van opslagproblemen]
  * [Problemen met de service controleren]
  * [Prestatieproblemen]
  * [Diagnose van fouten]
  * [Opslagproblemen emulator]
  * [Opslag Logboekregistratieprogramma 's]
  * [Met behulp van hulpprogramma's voor logboekregistratie]
* [End-to-end tracering]
  * [Logboekgegevens correleren]
  * [Clientaanvraag-ID]
  * [Server aanvraag-ID]
  * [Timestamps]
* [Hulp bij het oplossen van problemen]
  * [Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]
  * [Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]
  * [Prestatiegegevens geven hoge AverageServerLatency aan]
  * [U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]
  * [Prestatiegegevens geven een toename in PercentThrottlingError aan]
  * [Prestatiegegevens geven een toename in PercentTimeoutError aan]
  * [Prestatiegegevens geven een toename in PercentNetworkError aan]
  * [De client ontvangt HTTP 403-meldingen (verboden)]
  * [De client ontvangt HTTP 404-meldingen (niet gevonden)]
  * [De client ontvangt berichten van de HTTP-409 (Conflict)]
  * [Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]
  * [Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]
  * [Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]
  * [U ondervindt problemen met het installeren van de Azure SDK voor .NET]
  * [U hebt een ander probleem met storage-service]
  * [Oplossen van problemen met VHD's op Windows-machines](../../virtual-machines/windows/troubleshoot-vhds.md)   
  * [Oplossen van problemen met VHD's op virtuele Linux-machines](../../virtual-machines/linux/troubleshoot-vhds.md)
  * [Het oplossen van problemen met Windows Azure-bestanden](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Problemen met Azure Files oplossen met Linux](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [Bijlagen]
  * [Bijlage 1: Fiddler gebruiken om vast te leggen van HTTP en HTTPS-verkeer]
  * [Bijlage 2: Met behulp van Wireshark om vast te leggen van netwerkverkeer]
  * [Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer]
  * [Bijlage 4: Met behulp van Excel weergeven van metrische gegevens en logboekgegevens]
  * [Bijlage 5: Bewaking met Application Insights voor Azure DevOps]

## <a name="introduction"></a>Inleiding
Deze handleiding wordt beschreven hoe u functies zoals Azure Opslaganalyse, logboekregistratie in de Azure Storage-clientbibliotheek en andere hulpprogramma's van derden om te identificeren, onderzoeken en oplossen van Azure Storage client-side ' problemen met betrekking tot.

![][1]

Deze handleiding is bedoeld om te lezen voornamelijk gekenmerkt door de ontwikkelaars van online services die gebruikmaken van Azure Storage-Services en IT-professionals die verantwoordelijk is voor het beheer van dergelijke online services. De doelstellingen van deze handleiding zijn:

* Om u te helpen de status en prestaties van uw Azure Storage-accounts beheren.
* Om u te bieden de benodigde processen en hulpprogramma's om te bepalen of een probleem of een probleem in een toepassing is gekoppeld aan Azure Storage.
* Om u te bieden praktische richtlijnen voor het oplossen van problemen met betrekking tot Azure Storage.

### <a name="how-this-guide-is-organized"></a>Hoe deze handleiding is georganiseerd
De sectie '[Bewaking van uw storage-service]' wordt beschreven hoe u voor het bewaken van de status en prestaties van uw Azure Storage-services met behulp van Azure Storage Analytics Metrics (metrische gegevens van Storage).

De sectie '[diagnose van opslagproblemen]' wordt beschreven hoe u problemen identificeren met behulp van Azure Storage Analytics Logging (logboekregistratie van opslag). Ook wordt beschreven hoe u met behulp van de faciliteiten in één van de clientbibliotheken, zoals de Storage-clientbibliotheek voor .NET of de Azure SDK voor Java-client-side-logboekregistratie inschakelen.

De sectie '[End-to-end tracering]' wordt beschreven hoe u de gegevens in verschillende logboekbestanden en metrische gegevens kan correleren.

De sectie '[Hulp bij het oplossen van problemen]' biedt richtlijnen voor probleemoplossing voor sommige van de algemene opslag-gerelateerde zaken waarmee u kunt tegenkomen.

De '[bijlagen]"bevatten informatie over het gebruik van andere hulpprogramma's zoals Wireshark en Netmon gebruiken voor het analyseren van de pakketgegevens, Fiddler voor het analyseren van HTTP/HTTPS-berichten, het netwerk en Microsoft Message Analyzer voor het correleren van gegevens vastleggen.

## <a name="monitoring-your-storage-service"></a>Bewaking van uw storage-service
Als u bekend bent met het Windows-prestaties controleren, kunt u metrische gegevens van Storage zien als een Azure Storage-equivalent van de Prestatiemeter van Windows. In metrische opslaggegevens vindt u een uitgebreide set met metingen (tellers in Prestatiemeter van Windows-terminologie), zoals servicebeschikbaarheid, totaal aantal aanvragen voor de service of percentage van geslaagde aanvragen voor de service. Zie voor een volledige lijst van de beschikbare metrische gegevens, [tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx). U kunt opgeven of u wilt dat de storage-service voor het verzamelen en samenvoegen van metrische gegevens over elk uur of elke minuut. Zie voor meer informatie over het inschakelen van metrische gegevens en bewaken van uw opslagaccounts [metrische opslaggegevens inschakelen en weergeven van metrische gegevens](https://go.microsoft.com/fwlink/?LinkId=510865).

U kunt kiezen welke per uur metrische gegevens die u weergeven wilt de [Azure-portal](https://portal.azure.com) en regels configureren die beheerders per e-mail wordt gewaarschuwd wanneer er een uurtarief metrische gegevens een bepaalde drempelwaarde overschrijdt. Zie voor meer informatie, [waarschuwingsmeldingen ontvangen](/azure/monitoring-and-diagnostics/monitoring-overview-alerts). 

De storage-service verzamelt metrische gegevens met behulp van een best-effort, maar u kunt elke opslagbewerking niet registreren.

U kunt metrische gegevens zoals beschikbaarheid, totaal aantal aanvragen en gemiddelde latentie cijfers voor een opslagaccount weergeven in de Azure-portal. Een meldingsregel heeft een beheerder waarschuwen als beschikbaarheid onder een bepaald niveau komt ook is ingesteld. Deze gegevens kan bekijken, is een mogelijke gebied voor onderzoek slagingspercentage van de tabel-service wordt dan 100% (voor meer informatie, Zie de sectie '[Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]').

Uw Azure-toepassingen om te zorgen dat ze zijn in orde zijn en goed zoals verwacht, moet u continu controleren:

* Tot stand brengen van enkele prestatiegegevens voor een basislijn voor de toepassing waarmee u huidige gegevens vergelijken en alle belangrijke wijzigingen in het gedrag van Azure storage en uw toepassing te identificeren. De waarden van uw basislijn metrische gegevens, in veel gevallen is toepassingsspecifiek en moet u deze instellen wanneer u de prestaties van uw toepassing testen bent.
* Minuut metrische gegevens op te nemen en het gebruik ervan om te controleren actief onverwachte fouten en afwijkingen zoals pieken in het foutbericht telt, of vraag tarieven.
* Per uur metrische gegevens op te nemen en het gebruik ervan voor het bewaken van gemiddelde waarden, zoals gemiddelde aantallen van de fout en aanvraagsnelheden.
* Onderzoeken van mogelijke problemen met behulp van diagnostische hulpprogramma's zoals verderop beschreven in de sectie '[diagnose van opslagproblemen]. "

De grafieken in de volgende afbeelding ziet u hoe de gemiddelde die wordt uitgevoerd voor per uur metrische gegevens kan worden verborgen pieken in de activiteit. De metrische gegevens die per uur worden weergegeven om weer te geven van een constante snelheid van aanvragen, terwijl de minuut metrische gegevens geven de fluctuaties die echt plaatsvinden.

![][3]

De rest van deze sectie wordt beschreven welke metrische gegevens, moet u controleren en waarom.

### <a name="monitoring-service-health"></a>Servicestatus controleren
U kunt de [Azure-portal](https://portal.azure.com) om weer te geven van de status van de Storage-service (en andere Azure-services) in alle Azure-regio's over de hele wereld. Controle kunt u direct zien als een probleem buiten het besturingselement is die betrekking hebben op de Storage-service in de regio die u voor uw toepassing gebruiken.

De [Azure-portal](https://portal.azure.com) biedt ook meldingen van incidenten die invloed hebben op de verschillende Azure-services.
Opmerking: Deze informatie eerder beschikbaar was, samen met historische gegevens, op de [servicedashboard van Azure](http://status.azure.com).

Terwijl de [Azure-portal](https://portal.azure.com) verzamelt gegevens over de servicestatus van binnen de datacenters van Azure (binnen-out bewaking), u kunt ook heeft een buiten-in-benadering voor het genereren van synthetische transacties die regelmatig toegang hebben tot uw Azure gehoste web-App vanaf meerdere locaties. De services die worden aangeboden door [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) en Application Insights voor Azure DevOps zijn voorbeelden van deze benadering. Zie voor meer informatie over Application Insights voor Azure DevOps, de bijlage '[bijlage 5: Bewaking met Application Insights voor Azure DevOps](#appendix-5). "

### <a name="monitoring-capacity"></a>Bewaking van capaciteit
Metrische gegevens van Storage slaat alleen metrische gegevens over capaciteit voor de blob-service omdat blobs doorgaans rekening voor het grootste deel van de opgeslagen gegevens (op het moment van schrijven, dit is niet mogelijk met gebruik van metrische gegevens van Storage voor het bewaken van de capaciteit van uw tabellen en wachtrijen). U vindt deze gegevens in de **$MetricsCapacityBlob** tabel als u bewaking voor de Blob-service hebt ingeschakeld. Metrische opslaggegevens registreert u deze gegevens eenmaal per dag en kunt u de waarde van de **RowKey** om te bepalen of de rij bevat een entiteit met betrekking tot gebruikersgegevens (waarde **gegevens**) of analytics-gegevens (waarde **analytics**). Elke entiteit opgeslagen bevat informatie over de hoeveelheid opslag die wordt gebruikt (**capaciteit** gemeten in bytes) en het huidige aantal containers (**ContainerCount**) en blobs (**ObjectCount** ) in het opslagaccount dat wordt gebruikt. Voor meer informatie over de metrische gegevens over de capaciteit die zijn opgeslagen in de **$MetricsCapacityBlob** tabel, Zie [tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Deze waarden voor een vroegtijdige waarschuwing dat u de capaciteitslimieten van uw opslagaccount nadert, moet u controleren. In de Azure-portal, kunt u regels voor waarschuwingen om u te waarschuwen als de cumulatieve opslaggebruik overschrijden of minder is dan drempelwaarden die u opgeeft kunt toevoegen.
> 
> 

Voor meer informatie over het schatten van de grootte van verschillende storage-objecten, zoals blobs, Zie het blogbericht [inzicht in Azure Storage Billing-bandbreedte, transacties en capaciteit](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Bewaken van de beschikbaarheid
U moet de beschikbaarheid van de storage-services in uw opslagaccount controleren door de bewaking van de waarde in de **beschikbaarheid** kolom in de tabellen per uur of minuut metrische gegevens: **$MetricsHourPrimaryTransactionsBlob** , **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob** , **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. De **beschikbaarheid** kolom bevat een percentagewaarde die aangeeft van de beschikbaarheid van de service of de API-bewerking die wordt vertegenwoordigd door de rij (de **RowKey** bevat als de rij bevat metrische gegevens voor de service als geheel of voor een bepaalde API-bewerking).

Een waarde op die kleiner is dan 100% geeft aan dat sommige opslag-aanvragen mislukken. U kunt zien waarom ze mislukken door te controleren van de andere kolommen in de metrische gegevens waarin het aantal aanvragen met verschillende typen zoals **ServerTimeoutError**. U mag verwachten **beschikbaarheid** tijdelijk vallen onder de 100% om redenen als tijdelijke server time-outs tijdens de service wordt verplaatst partities naar betere taakverdeling op aanvraag; de logica voor opnieuw proberen in uw clienttoepassing moet onregelmatige voorwaarden worden verwerkt. Het artikel [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) geeft een lijst van de transactietypen die metrische gegevens van Storage bevat in de **beschikbaarheid** berekening.

In de [Azure-portal](https://portal.azure.com), kunt u regels voor waarschuwingen om u te waarschuwen als toevoegen **beschikbaarheid** voor een service valt onder de drempelwaarde die u opgeeft.

De '[Hulp bij het oplossen van problemen]' van deze handleiding beschrijft enkele veelvoorkomende storage service-problemen met betrekking tot beschikbaarheid.

### <a name="monitoring-performance"></a>Prestaties bewaken
Voor het controleren van de prestaties van de storage-services, kunt u de volgende metrische gegevens uit de tabellen voor metrische gegevens per uur en minuut.

* De waarden in de **AverageE2ELatency** en **averageserverlatency aan** kolommen bevatten de gemiddelde tijd van de storage-service of type API-bewerking duurt naar aanvragen verwerkt. **AverageE2ELatency** is een maateenheid voor end-to-end-latentie met de benodigde tijd voor het lezen van de aanvraag en het antwoord naast de benodigde tijd voor het verwerken van de aanvraag te verzenden (dus inclusief netwerklatentie zodra de aanvraag voor de opslag bereikt Service). **Averageserverlatency aan** is een meting van alleen de verwerkingstijd en daarom niet van toepassing op alle netwerklatentie die betrekking hebben op de client communiceert. Zie de sectie '[prestatiegegevens geven hoge AverageE2ELatency en lage averageserverlatency aan]' verderop in deze handleiding voor een discussie over waarom er mogelijk een belangrijk verschil tussen deze twee waarden.
* De waarden in de **TotalIngress** en **TotalEgress** kolommen bevatten de totale hoeveelheid gegevens in bytes, die bij binnenkomen en gaan uit uw storage-service of via een bepaald type van de API-bewerking.
* De waarden in de **TotalRequests** kolom tonen het totale aantal aanvragen dat is ontvangen van de storage-service van de API-bewerking. **TotalRequests** is het totale aantal aanvragen die de storage-service ontvangt.

Normaal gesproken bewaakt u onverwachte wijzigingen in een van deze waarden als een indicator dat er een probleem waarvoor onderzoek.

In de [Azure-portal](https://portal.azure.com), kunt u regels voor waarschuwingen om u te waarschuwen als een van de maatstaven voor prestaties voor deze service bij minder dan of groter zijn dan een drempel die u opgeeft toevoegen.

De '[Hulp bij het oplossen van problemen]' van deze handleiding beschrijft enkele veelvoorkomende storage service-problemen die betrekking hebben op prestaties.

## <a name="diagnosing-storage-issues"></a>Diagnose van opslagproblemen
Er zijn een aantal manieren die u mogelijk op de hoogte van een probleem of een probleem in uw toepassing, met inbegrip van:

* Een ernstige fout die ervoor zorgt de toepassing dat vastloopt of niet meer werken.
* Belangrijke wijzigingen van basislijnwaarden in de metrische gegevens die u controleren wilt, zoals beschreven in de vorige sectie "[Bewaking van uw storage-service]."
* Rapporten van gebruikers van uw toepassing die een bepaalde bewerking is niet voltooid zoals verwacht of dat bepaalde functie niet werkt.
* Fouten die zijn gegenereerd in uw toepassing die worden weergegeven in logboekbestanden of via een andere meldingsmethode.

Problemen met betrekking tot Azure storage-services kunnen meestal worden onderverdeeld in vier hoofdcategorieën:

* Uw toepassing heeft een prestatieprobleem, zoals gemeld door uw gebruikers of aan het licht komt door wijzigingen in de maatstaven voor prestaties.
* Er is een probleem met de Azure Storage-infrastructuur in een of meer regio's.
* Uw toepassing is er een fout optreedt, zoals gemeld door uw gebruikers of aan het licht komt door een toename in een van de fout aantal metrische gegevens die u wilt bewaken.
* Tijdens de ontwikkeling en testen kunt u de emulator van de lokale opslag; gebruikt u kunt enkele problemen die specifiek betrekking op het gebruik van de opslagemulator hebben tegenkomen.

De volgende gedeelten worden de stappen die u moet volgen om te onderzoeken en oplossen van problemen in elk van deze vier categorieën. De sectie '[Hulp bij het oplossen van problemen]' verderop in deze handleiding vindt u meer details over voor enkele veelvoorkomende problemen die kunnen optreden.

### <a name="service-health-issues"></a>Problemen met de service controleren
Problemen met de status van de service zijn doorgaans buiten het besturingselement. De [Azure-portal](https://portal.azure.com) vindt u informatie over eventuele lopende problemen met Azure-services zoals storage-services. Als u ervoor hebt gekozen voor geografisch redundante opslag met leestoegang tijdens het maken van uw storage-account, kunt klikt u vervolgens als uw gegevens niet beschikbaar is op de primaire locatie, uw toepassing overschakelen tijdelijk naar de alleen-lezen kopie op de secundaire locatie. Als u wilt lezen van de secundaire, moet uw toepassing kunnen schakelen tussen het gebruik van de primaire en secundaire opslaglocaties en kunnen werken in een modus met verminderde functionaliteit met alleen-lezen gegevens. De Azure Storage-clientbibliotheken kunnen u voor het definiëren van een beleid voor opnieuw proberen dat uit de secundaire opslag lezen kan als een leesbewerking vanuit de primaire opslag is mislukt. Uw toepassing moet ook rekening mee dat de gegevens in de secundaire locatie uiteindelijk consistent is. Zie voor meer informatie het blogbericht [redundantieopties voor Azure-opslag en geografisch redundante opslag met leestoegang](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Prestatieproblemen
De prestaties van een toepassing kunnen subjectief zijn, met name vanuit het perspectief van een gebruiker. Het is daarom belangrijk dat u over prestatiegegevens voor een basislijn beschikt aan de hand waarvan u kunt bepalen waar er prestatieproblemen zijn. Veel factoren kunnen invloed hebben op de prestaties van een Azure storage-service vanuit het perspectief van de toepassing client. Deze factoren werkt mogelijk in de storage-service, in de client of in de netwerkinfrastructuur; Daarom is het belangrijk dat u hebt een strategie voor het identificeren van de oorsprong van het prestatieprobleem.

Nadat u de waarschijnlijk locatie van de oorzaak van het prestatieprobleem van de metrische gegevens hebt geïdentificeerd, kunt u de logboekbestanden vervolgens gebruiken om gedetailleerde informatie te diagnosticeren en oplossen van het probleem verder te zoeken.

De sectie '[Hulp bij het oplossen van problemen]' verderop in deze handleiding vindt u meer informatie over enkele veelvoorkomende problemen met betrekking tot prestaties kunnen optreden.

### <a name="diagnosing-errors"></a>Diagnose van fouten
Gebruikers van uw toepassing kunnen melding van fouten die zijn gerapporteerd door de clienttoepassing. Metrische opslaggegevens registreert ook de aantallen van verschillende typen van uw storage-services zoals **NetworkError**, **ClientTimeoutError**, of **AuthorizationError**. Terwijl metrische opslaggegevens alleen tellingen van verschillende typen records, kunt u meer informatie over afzonderlijke aanvragen verkrijgen door de serverzijde en clientzijde netwerklogboeken onderzoeken. De HTTP-statuscode geretourneerd door de storage-service krijgt doorgaans een indicatie van waarom de aanvraag is mislukt.

> [!NOTE]
> Houd er rekening mee dat u kunt verwachten om te zien van bepaalde onregelmatige fouten: bijvoorbeeld fouten als gevolg van tijdelijke netwerkproblemen of toepassingsfouten.
> 
> 

De volgende resources zijn nuttig voor het begrijpen van opslag met betrekking tot de status en foutcodes:

* [Algemene REST-API-foutcodes](https://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Foutcodes voor blob-services](https://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Foutcodes voor wachtrij](https://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Foutcodes voor tabel](https://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Foutcodes voor bestand](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Opslagproblemen emulator
De Azure-SDK bevat een opslagemulator die u op een werkstation ontwikkeling uitvoeren kunt. Deze emulator simuleert de meeste van de werking van de Azure storage-services en is handig tijdens het ontwikkelen en testen, zodat u toepassingen die gebruikmaken van Azure storage-services zonder de noodzaak van een Azure-abonnement en Azure storage-account uit te voeren.

De '[Hulp bij het oplossen van problemen]' van deze handleiding beschrijft enkele veelvoorkomende problemen aangetroffen met behulp van de opslagemulator.

### <a name="storage-logging-tools"></a>Opslag Logboekregistratieprogramma 's
Logboekregistratie van opslag biedt logboekregistratie op de server van aanvragen voor opslag in uw Azure storage-account. Zie voor meer informatie over het inschakelen van logboekregistratie op de server en toegang krijgen tot de logboekgegevens [vastleggen van Storage inschakelen en toegang krijgen tot logboekgegevens](https://go.microsoft.com/fwlink/?LinkId=510867).

De Opslagclientbibliotheek voor .NET kunt u voor het verzamelen van gegevens van een client-side met betrekking tot opslagbewerkingen die worden uitgevoerd door uw toepassing. Zie [Logboekregistratie op de client inschakelen met de .NET-opslagclientbibliotheek](https://go.microsoft.com/fwlink/?LinkId=510868) voor meer informatie.

> [!NOTE]
> In sommige gevallen (zoals autorisatiefouten SAS), kan een gebruiker waarvoor u geen aanvraaggegevens in de logboeken van de opslag-serverzijde vinden kunt fout gemeld. U kunt de mogelijkheden voor logboekregistratie van het Storage-clientbibliotheek gebruiken voor het onderzoeken van als de oorzaak van het probleem op de client is of netwerk controlehulpprogramma's gebruiken voor het onderzoeken van het netwerk.
> 
> 

### <a name="using-network-logging-tools"></a>Met behulp van hulpprogramma's voor logboekregistratie
U kunt het verkeer tussen de client en server voor gedetailleerde informatie over de gegevens die de client en server uitwisselt en de onderliggende netwerkomstandigheden vastleggen. Nuttige hulpprogramma's voor registratie zijn onder andere:

* [Fiddler](http://www.telerik.com/fiddler) is een gratis proxy waarmee u de kopteksten en gegevens over de nettolading van HTTP en HTTPS-aanvraag en antwoord-berichten te onderzoeken voor Webfoutopsporing. Zie voor meer informatie, [bijlage 1: Fiddler gebruiken om vast te leggen van HTTP en HTTPS-verkeer](#appendix-1).
* [Microsoft Network Monitor (Netmon)](https://www.microsoft.com/download/details.aspx?id=4865) en [Wireshark](http://www.wireshark.org/) zijn gratis netwerk protocol-analyse waarmee u kunt gedetailleerde pakket weergeven voor een breed scala van netwerkprotocollen. Zie voor meer informatie over Wireshark "[bijlage 2: Met behulp van Wireshark om vast te leggen netwerkverkeer](#appendix-2)'.
* Microsoft Message Analyzer is een hulpprogramma van Microsoft die Netmon en die naast het vastleggen van gegevens van het pakket netwerk vervangt, helpt u bij het weergeven en analyseren van de logboekgegevens die zijn vastgelegd met andere hulpprogramma's. Zie voor meer informatie "[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer](#appendix-3)'.
* Als u een basisconnectiviteit test uitvoeren om te controleren wilt dat de clientcomputer verbinding met de Azure storage-service via het netwerk maken kan, dit niet mogelijk met behulp van de standaard **ping** hulpprogramma op de client. Echter, kunt u de [ **tcping** hulpprogramma](http://www.elifulkerson.com/projects/tcping.php) om te controleren op connectiviteit.

In veel gevallen de logboekgegevens van logboekregistratie van opslag en de Storage-clientbibliotheek is voldoende om het probleem vast te stellen, maar in sommige scenario's, moet u mogelijk de meer gedetailleerde informatie die deze hulpprogramma's voor network logboekregistratie kunnen bieden. Bijvoorbeeld, kunt u header en -nettolading inzien verzonden naar en van de storage-services, waarmee u bekijken hoe een clienttoepassing opslagbewerkingen pogingen met Fiddler om HTTP en HTTPS-berichten te bekijken. Protocol analyzers zoals Wireshark worden uitgevoerd op het pakketniveau van het zodat u kunt bekijken van TCP-gegevens, waarmee u problemen met verloren pakketten en problemen met de netwerkverbinding. Message Analyzer kan worden uitgevoerd op zowel HTTP- en TCP-lagen.

## <a name="end-to-end-tracing"></a>End-to-end tracering
End-to-end tracering met behulp van verschillende logboekbestanden is een handige techniek voor het onderzoeken van mogelijke problemen. U kunt de datum/tijd-gegevens gebruiken uit uw metrische gegevens als een indicatie van waar u wilt beginnen met zoeken in de logboekbestanden voor de gedetailleerde informatie waarmee u het probleem op te lossen.

### <a name="correlating-log-data"></a>Logboekgegevens correleren
Bij het weergeven van Logboeken van clienttoepassingen netwerk wordt getraceerd en logboekregistratie van de server-side-opslag is het belangrijk om te kunnen correleren aanvragen via de verschillende logboekbestanden. De logboekbestanden van opnemen een aantal verschillende velden die handig als de correlatie-id's zijn. De clientaanvraag-ID is het nuttigst veld gebruiken om te posten in de verschillende logboeken correleren. Maar in sommige gevallen, kan het nuttig zijn om de server aanvraag-ID of de tijdstempels te gebruiken. De volgende secties vindt u meer informatie over deze opties.

### <a name="client-request-id"></a>Clientaanvraag-ID
De Opslagclientbibliotheek genereert automatisch een unieke client-aanvraag-ID voor elke aanvraag.

* In het logboek van clientzijde die de Storage-clientbibliotheek maakt, de clientaanvraag-ID wordt weergegeven in de **clientaanvraag-ID** veld van elke logboekvermelding met betrekking tot de aanvraag.
* In een netwerktracering zoals een vastgelegd door Fiddler, de clientaanvraag-ID wordt weergegeven in de berichten aanvragen als de **x-ms-client-request-id** HTTP-header-waarde.
* In het logboek serverzijde logboekregistratie van opslag, is de clientaanvraag-ID wordt weergegeven in de kolom-ID van Client-aanvraag.

> [!NOTE]
> Het is mogelijk voor meerdere aanvragen voor delen de dezelfde clientaanvraag-ID omdat de client deze waarde toewijzen kan (Hoewel de Storage-clientbibliotheek automatisch een nieuwe waarde wijst). Wanneer de client opnieuw probeert, delen alle pogingen dezelfde client aanvraag-ID. In het geval van een batch verzonden vanaf de client, heeft de batch een aanvraag-id van één client.
> 
> 

### <a name="server-request-id"></a>Server aanvraag-ID
Server aanvraag-id's wordt automatisch gegenereerd door de storage-service.

* In het logboek serverzijde logboekregistratie van opslag, de aanvraag-ID wordt weergegeven de **aanvraag-ID-header** kolom.
* In een netwerktracering zoals een vastgelegd door Fiddler, de aanvraag-ID wordt weergegeven in antwoordberichten als de **x-ms-request-id** HTTP-header-waarde.
* In het logboek van clientzijde die de Storage-clientbibliotheek maakt, de aanvraag-ID wordt weergegeven in de **bewerking tekst** kolom voor de vermelding van de details van de reactie van de server.

> [!NOTE]
> De storage-service altijd een unieke server aanvraag-ID aan toegewezen elke aanvraag die wordt ontvangen, zodat elke nieuwe poging van de client en elke bewerking die is opgenomen in een batch een unieke server aanvraag-ID heeft.
> 
> 

Als de Storage-clientbibliotheek genereert een **StorageException** in de client, de **RequestInformation** eigenschap bevat een **RequestResult** -object met een  **ServiceRequestID** eigenschap. U hebt ook toegang tot een **RequestResult** object uit een **OperationContext** exemplaar.

Het volgende codevoorbeeld laat zien hoe u om in te stellen van een aangepaste **ClientRequestId** waarde door het koppelen van een **OperationContext** object van de aanvraag voor de storage-service. Ook ziet u hoe om op te halen de **ServerRequestId** waarde van het antwoordbericht.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within the application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded to file: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due to client side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a>Tijdstempels
U kunt ook tijdstempels naar gerelateerde vermeldingen vinden, maar zorg ervoor dat u van een klok scheeftrekken tussen de client en server die mogelijk aanwezig zijn. Zoeken naar plus of min 15 minuten voor het afstemmen van server-side-vermeldingen op basis van de tijdstempel op de client. Houd er rekening mee dat de metagegevens van de blob voor de blobs met metrische gegevens geeft aan het tijdsbereik voor de metrische gegevens die zijn opgeslagen in de blob dat. Dit tijdsbereik is handig als er veel blobs zijn metrische gegevens voor dezelfde minuut of uur.

## <a name="troubleshooting-guidance"></a>Richtlijnen voor probleemoplossing
In deze sectie helpt u bij de diagnose en het oplossen van enkele van de problemen die uw toepassing kan optreden bij het gebruik van de Azure storage-services. Gebruik de onderstaande lijst om de informatie die relevant zijn voor uw specifieke probleem te vinden.

**Beslissingsstructuur voor het oplossen van problemen**

---
Uw probleem betrekking hebben op de prestaties van een van de storage-services?

* [Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]
* [Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]
* [Prestatiegegevens geven hoge AverageServerLatency aan]
* [U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]

---
Uw probleem betrekking hebben op de beschikbaarheid van een van de storage-services?

* [Prestatiegegevens geven een toename in PercentThrottlingError aan]
* [Prestatiegegevens geven een toename in PercentTimeoutError aan]
* [Prestatiegegevens geven een toename in PercentNetworkError aan]

---
 Is uw clienttoepassing een HTTP 4XX (zoals 404)-antwoord ontvangen van een storage-service?

* [De client ontvangt HTTP 403-meldingen (verboden)]
* [De client ontvangt HTTP 404-meldingen (niet gevonden)]
* [De client ontvangt berichten van de HTTP-409 (Conflict)]

---
[Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]

---
[Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]

---
[U ondervindt onverwacht opnieuw opstarten van virtuele Machines met een groot aantal gekoppelde VHD's]

---
[Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]

---
[U ondervindt problemen met het installeren van de Azure SDK voor .NET]

---
[U hebt een ander probleem met storage-service]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Prestatiegegevens geven hoge AverageE2ELatency en lage averageserverlatency aan
De afbeelding hieronder uit de [Azure-portal](https://portal.azure.com) hulpprogramma voor het controleren van toont een voorbeeld waarin de **AverageE2ELatency** is aanzienlijk hoger dan de **averageserverlatency aan**.

![][4]

Berekent de storage-service alleen de metriek **AverageE2ELatency** voor geslaagde aanvragen en, in tegenstelling tot **averageserverlatency aan**, bevat de tijd voor de client de gegevens verzenden en ontvangen bevestiging van de storage-service. Daarom een verschil tussen **AverageE2ELatency** en **averageserverlatency aan** kan worden door de clienttoepassing wordt traag reageren, hetzij vanwege het voorwaarden op het netwerk.

> [!NOTE]
> U kunt ook weergeven **E2ELatency** en **ServerLatency** gegevens vastleggen voor afzonderlijke opslagbewerkingen in de logboekregistratie van opslag.
> 
> 

#### <a name="investigating-client-performance-issues"></a>Het onderzoeken van prestatieproblemen van client
Mogelijke oorzaken voor de client reageert langzaam zijn met een beperkt aantal beschikbare verbindingen of threads of onvoldoende bronnen, zoals CPU, geheugen of netwerk bandbreedte wordt. U kunt mogelijk het probleem oplossen door het wijzigen van de clientcode voor het efficiënter (bijvoorbeeld door met behulp van asynchrone aanroepen met de storage-service) of met behulp van een grotere virtuele Machine (met meer kernen en meer geheugen).

Voor de tabel en wachtrij, het algoritme Nagle kan ook leiden tot hoog **AverageE2ELatency** in vergelijking met op **averageserverlatency aan**: Zie voor meer informatie het bericht [van Nagle Algoritme is niet beschrijvende voor kleine aanvragen](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). U kunt het algoritme Nagle in code uitschakelen met behulp van de **ServicePointManager** klasse de **System.Net** naamruimte. U moet dit doen voordat u de aanroepen voor de tabel of queue-services in uw toepassing omdat dit heeft geen invloed op verbindingen die al zijn geopend. Het volgende voorbeeld is afkomstig uit de **Application_Start** methode in een werkrol.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Controleer de logboeken van de client-side om te zien hoeveel aanvragen indienen van uw clienttoepassing en controle voor algemene .NET die betrekking hebben knelpunten in uw client, zoals CPU, .NET-garbagecollection, het netwerkgebruik of geheugen. Als een beginpunt voor het oplossen van .NET-clienttoepassingen, Zie [foutopsporing en tracering Profiling](https://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Netwerklatentieproblemen onderzoeken
Normaal gesproken komt hoge end-to-end latentie veroorzaakt door het netwerk door tijdelijke omstandigheden. U kunt beide tijdelijke en permanente netwerkproblemen zoals verloren gegane pakketten onderzoeken met behulp van hulpprogramma's zoals Wireshark of Microsoft Message Analyzer.

Zie voor meer informatie over het gebruiken van Wireshark voor het oplossen van netwerkproblemen, "[Bijlage 2: Met behulp van Wireshark om vast te leggen van netwerkverkeer]. "

Zie voor meer informatie over Microsoft Message Analyzer gebruiken voor het oplossen van netwerkproblemen, "[Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer]. "

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Prestatiegegevens geven lage AverageE2ELatency en lage averageserverlatency aan, maar de client ondervindt hoge latentie
In dit scenario is de meest waarschijnlijke oorzaak een vertraging in de opslag-aanvragen dat de opslagservice is bereikt. U moet onderzoeken waarom aanvragen van de client zijn niet beschikbaar tot en met de blobservice.

Een mogelijke reden voor de client vertraagd verzenden van aanvragen is dat er een beperkt aantal beschikbare verbindingen of threads.

Ook controleren of de client meerdere nieuwe pogingen uitvoert, en onderzoek de reden waarom als het. Om te bepalen of de client meerdere nieuwe pogingen uitvoert, kunt u het volgende doen:

* Bekijk de Opslaganalyse-Logboeken. Als meerdere pogingen zijn gebeurt, ziet u meerdere bewerkingen met de dezelfde clientaanvraag-ID, maar met verschillende serveraanvraag id's.
* Bekijk de clientlogboeken van de. Uitgebreide logboekregistratie wordt aangegeven dat er een nieuwe poging is opgetreden.
* Fouten opsporen in uw code en controleer de eigenschappen van de **OperationContext** object dat is gekoppeld aan de aanvraag. Als de bewerking opnieuw is uitgevoerd, de **RequestResults** eigenschap meerdere unieke serveraanvraag id's bevat. U kunt ook de begin- en eindtijden voor elke aanvraag controleren. Zie voor meer informatie de voorbeeldcode in de sectie [Server aanvraag-ID].

Als er geen problemen in de client, kunt u mogelijke netwerkproblemen zoals pakketverlies moet onderzoeken. U kunt hulpprogramma's zoals Wireshark of Microsoft Message Analyzer gebruiken voor het onderzoeken van netwerkproblemen.

Zie voor meer informatie over het gebruiken van Wireshark voor het oplossen van netwerkproblemen, "[Bijlage 2: Met behulp van Wireshark om vast te leggen van netwerkverkeer]. "

Zie voor meer informatie over Microsoft Message Analyzer gebruiken voor het oplossen van netwerkproblemen, "[Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer]. "

### <a name="metrics-show-high-AverageServerLatency"></a>Prestatiegegevens geven hoge averageserverlatency aan
In het geval van hoog **averageserverlatency aan** blob downloadaanvragen, moet u de logboekregistratie van opslag-Logboeken gebruiken om te zien of er herhaalde verzoeken voor de dezelfde blob (of een reeks blobs zijn). Voor blob uploaden-aanvragen, moet u onderzoeken welke blok grootte van de client is (bijvoorbeeld blokken kleiner is dan 64 kB groot kan resulteren in overhead, tenzij de leesbewerkingen zijn ook in minder dan 64 kB segmenten geüpload) gebruikt en als meerdere clients blokken uploadt naar dezelfde blob in para parallelle. Controleer ook de per minuut metrische gegevens voor pieken in het aantal aanvragen die leiden tot meer dan de per seconde schaalbaarheidsdoelen: Zie ook '[Prestatiegegevens geven een toename in PercentTimeoutError aan]. "

Als u hoge ziet **averageserverlatency aan** blob gedownload aanvragen wanneer er worden herhaald aanvragen dezelfde blob of set-blobs en vervolgens kunt u overwegen deze blobs met Azure-Cache of het leveren van inhoud Azure caching Network (CDN). Voor uploadaanvragen, kunt u de doorvoer verbeteren met behulp van een groter blok. Voor query's en tabellen is het ook mogelijk voor het implementeren van caching aan clientzijde op clients die dezelfde querybewerkingen uitvoeren en waar de gegevens niet regelmatig worden gewijzigd.

Hoge **averageserverlatency aan** waarden zijn ook een symptoom van een slecht ontworpen tabellen of query's resulteren in scanbewerkingen of die anti patroon toevoegen/toevoegen volgen. Zie voor meer informatie "[Prestatiegegevens geven een toename in PercentThrottlingError aan]'.

> [!NOTE]
> U kunt een uitgebreide controlelijst voor prestaties controlelijst hier vinden: [Prestaties van Microsoft Azure Storage en Controlelijst voor schaalbaarheid](storage-performance-checklist.md).
> 
> 

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij
Als u een vertraging tussen het moment dat een toepassing wordt een bericht aan een wachtrij en de tijd dat deze beschikbaar om te lezen uit de wachtrij voordoet zich, moet u de volgende stappen uit om het probleem vast te stellen uitvoeren:

* Controleer of dat de toepassing is met succes is met het toevoegen van de berichten naar de wachtrij. Controleer of de toepassing niet opnieuw te proberen de **AddMessage** methode meerdere keren voordat slaagt. Een herhaalde pogingen van opslagbewerkingen ziet u de logboeken van de Storage-clientbibliotheek.
* Controleer of er is geen klok scheeftrekken tussen de werkrol die het bericht toegevoegd aan de wachtrij en de werkrol die leest het bericht uit de wachtrij waarmee u kunt worden weergegeven alsof er is een vertraging in de verwerking.
* Controleer als de werkrol die de berichten kan uit de wachtrij lezen is mislukt. Als een client wachtrij roept de **GetMessage** methode, maar niet reageren met een bevestiging, het bericht blijft zichtbaar in de wachtrij staan totdat de **invisibilityTimeout** periode is verlopen. Het bericht wordt op dit moment beschikbaar zijn voor het verwerken van het opnieuw.
* Controleer als de wachtrijlengte na verloop van tijd toeneemt. Dit kan gebeuren als er niet voldoende werknemers beschikbaar voor het verwerken van alle berichten die andere werknemers in de wachtrij zijn geplaatst. Controleer ook de metrische gegevens om te zien als de delete-aanvragen mislukken en de wachtrij halen tellen op berichten, dit kunnen duiden op herhaalde mislukte pogingen om het bericht te verwijderen.
* Bekijk de logboekbestanden logboekregistratie van opslag voor alle wachtrijbewerkingen waarvoor hoger dan verwacht **E2ELatency** en **ServerLatency** waarden over een langere periode dan normaal.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Prestatiegegevens geven een toename in percentthrottlingerror aan
Beperkingsfouten optreden wanneer u de schaalbaarheidsdoelen van een opslagservice overschrijdt. Zorg ervoor dat geen enkele client of tenant vertragingen in Services opslag kunnen gebruikmaken van de service ten koste van anderen. Zie voor meer informatie, [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie over de schaalbaarheidsdoelen voor storage-accounts en prestatiedoelen voor partities binnen storage-accounts.

Als de **percentthrottlingerror aan** metrische gegevens geven een toename in het percentage aanvragen dat met een beperking-fout optreedt, moet u voor het onderzoeken van een van twee scenario's:

* [Tijdelijke toename in percentthrottlingerror aan]
* [Permanente toename in percentthrottlingerror aan fout]

Een toename van **percentthrottlingerror aan** vaak optreedt op hetzelfde moment als een toename van het aantal aanvragen voor opslag, of wanneer u in eerste instantie bent laden van uw toepassing testen. Dit kan ook zelf in de client als '503 Server bezet' of '500 time-out van de bewerking' http-statusberichten-opslagbewerkingen manifest.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tijdelijke toename in percentthrottlingerror aan
Als er pieken in de waarde van **percentthrottlingerror aan** die overeenkomen met perioden met veel activiteit voor de toepassing, het implementeren van een (niet-lineaire) uitstel strategie voor exponentieel voor nieuwe pogingen in uw client. Back-off nieuwe pogingen de onmiddellijke belasting van de partitie te verminderen en zodat uw toepassing te gebruiken om pieken in verkeer. Zie voor meer informatie over het implementeren van beleid voor opnieuw proberen met behulp van de Storage-clientbibliotheek [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [!NOTE]
> U ziet ook pieken in de waarde van **percentthrottlingerror aan** die niet samenvallen met perioden met veel activiteit voor de toepassing: de meest waarschijnlijke oorzaak hier is de storage-service voor het verplaatsen van partities voor het verbeteren van de taakverdeling.
> 
> 

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanente toename in percentthrottlingerror aan fout
Als er een consistent hoge waarde voor **percentthrottlingerror aan** na een permanente toename in uw transactie-volumes, of als u bij het uitvoeren van uw eerste load tests op uw toepassing, moet u om te evalueren hoe uw toepassing maakt gebruik van opslag-partities en of deze nadert de prestatiedoelen voor een opslagaccount. Bijvoorbeeld, als er beperkingsfouten optreden in een wachtrij (die telt als één partitie), moet klikt u vervolgens u overwegen aanvullende wachtrijen op de transacties verdeeld over meerdere partities. Als er beperkingsfouten optreden in een tabel, moet u overwegen om een andere partitieschema transacties verspreiden over meerdere partities met behulp van een groot aantal partitiesleutelwaarden die zijn. Een veelvoorkomende oorzaak van dit probleem is de prepend/append anti-patroon waar u de datum selecteren als de partitiesleutel en vervolgens alle gegevens op een bepaalde dag wordt opgeslagen in één partitie: belast, kan dit resulteren in een knelpunt met schrijven. Houd rekening met een ander partitioneren ontwerp of evalueren of met behulp van blob-opslag is mogelijk een betere oplossing. Ook controleren of beperking als gevolg van pieken in verkeer voordoet zich en manieren om aanpassing van het patroon van aanvragen te onderzoeken.

Als u uw transacties over meerdere partities verdelen, moet u nog steeds op de hoogte van de schaalbaarheidslimieten instellen voor het opslagaccount zijn. Bijvoorbeeld, als u tien wachtrijen elke het maximum van 2000 1KB berichten per seconde verwerken gebruikt, kunt u zich bij de totale limiet van 20.000 berichten per seconde voor het opslagaccount. Als u nodig hebt voor het verwerken van meer dan 20.000 entiteiten per seconde, moet u overwegen meerdere opslagaccounts. U moet ook Houd er rekening mee dat de grootte van uw aanvragen en entiteiten een invloed heeft op wanneer de storage-service de clients vertraagt: als u grotere aanvragen en entiteiten hebt, u sneller kan worden beperkt.

Inefficiënt Queryontwerp kan ook leiden tot u om de limieten voor schaalbaarheid voor de Tabelpartities. Bijvoorbeeld, moet een query met een filter selecteert die alleen één procent van de entiteiten in een partitie, maar die alle entiteiten in een partitie gescand toegang tot elke entiteit. Elke entiteit lezen tellen mee voor het totale aantal transacties in de betreffende partitie; Daarom kunt u eenvoudig de schaalbaarheidsdoelen bereiken.

> [!NOTE]
> Uw prestatietests, zou moeten uitwijzen inefficiënt queryontwerpen in uw toepassing.
> 
> 

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Prestatiegegevens geven een toename in percenttimeouterror aan
Uw metrische gegevens geven een toename in **percenttimeouterror aan** voor een van uw storage-services. Op hetzelfde moment ontvangt de client een groot aantal statusberichten '500 time-out van de bewerking' http-van-opslagbewerkingen.

> [!NOTE]
> Mogelijk ziet u time-outfouten optreden als de storage-service tijdelijk laden saldo-aanvragen door een partitie verplaatsen naar een nieuwe server.
> 
> 

De **percenttimeouterror aan** meetwaarde is een samenvoeging van de volgende metrische gegevens: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**,  **AnonymousServerTimeoutError**, en **SASServerTimeoutError**.

De server-time-outs worden veroorzaakt door een fout op de server. De clienttime-outs gebeuren wanneer een bewerking op de server heeft de time-out die is opgegeven door de client; overschreden bijvoorbeeld, een client met de Storage-clientbibliotheek is een time-out voor een bewerking kunt instellen met behulp van de **ServerTimeout** eigenschap van de **QueueRequestOptions** klasse.

Time-outs van de server duiden op een probleem met de storage-service die verder onderzoek vereist. U kunt metrische gegevens gebruiken om te zien als u de schaalbaarheid de limiet hebt bereikt voor de service en eventuele pieken in verkeer dat dit probleem mogelijk veroorzaakt door identificeren. Als het probleem onregelmatig is, dit kan worden veroorzaakt door load balancing-activiteit in de service. Als het probleem permanent is en niet kan worden veroorzaakt door uw toepassing te maken met de schaalbaarheidslimieten van de service, moet u een Ondersteuningsprobleem verhogen. Voor clienttime-outs, moet u bepalen als de time-out is ingesteld op een passende waarde in de client en een van de wijzigingen de time-outwaarde in de client instellen of onderzoeken hoe u kunt de prestaties verbeteren van de bewerkingen in de storage-service, bijvoorbeeld door te optimaliseren uw tabel query's of de grootte van uw berichten beperken.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Prestatiegegevens geven een toename in percentnetworkerror aan
Uw metrische gegevens geven een toename in **percentnetworkerror aan** voor een van uw storage-services. De **percentnetworkerror aan** meetwaarde is een samenvoeging van de volgende metrische gegevens: **NetworkError**, **AnonymousNetworkError**, en **SASNetworkError**. Deze zich voordoen wanneer de storage-service een fout wordt gedetecteerd als de client een aanvraag voor opslag.

De meest voorkomende oorzaak van deze fout is een client verbinding verbreken voordat een time-out is verlopen in de storage-service. Onderzoek de code in uw client om te begrijpen waarom en wanneer de client wordt losgekoppeld van de storage-service. U kunt ook Wireshark, Microsoft Message Analyzer of Tcping gebruiken voor het onderzoeken van problemen met de netwerkverbinding van de client. Deze hulpprogramma's worden beschreven in de [bijlagen].

### <a name="the-client-is-receiving-403-messages"></a>De client ontvangt berichten van HTTP 403 (verboden)
Als u de clienttoepassing HTTP 403 (verboden) fouten genereren wordt, is een waarschijnlijke oorzaak dat de client een verlopen Shared Access Signature (SAS) is gebruikt tijdens het verzenden van een aanvraag voor opslag (Hoewel andere mogelijke oorzaken klok scheeftrekken, ongeldig sleutels en leeg kopteksten zijn ). Als een verlopen SAS-sleutel voor de oorzaak is, ziet u niet alle vermeldingen in de logboekgegevens van server-side logboekregistratie van opslag. De volgende tabel ziet u een voorbeeld van de client-side-logboekbestanden die worden gegenereerd door de Storage-clientbibliotheek die laat dit probleem optreedt zien:

| Bron | Uitgebreidheid | Uitgebreidheid | Clientaanvraag-id | Bewerking tekst |
| --- | --- | --- | --- | --- |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-… |Bewerking met de primaire locatie per locatiemodus PrimaryOnly starten. |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab -… |Synchrone verzoek om te beginnen https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp; sr = c&amp;si = mypolicy&amp;sig OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % = 3D&amp;api-versie 2014-02-14 =. |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab -… |Wachten op antwoord. |
| Microsoft.WindowsAzure.Storage |Waarschuwing |2 |85d077ab -… |Uitzondering geretourneerd tijdens het wachten op reactie: De externe server heeft een fout geretourneerd: (403) Verboden. |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab -… |Antwoord ontvangen. Statuscode 403, aanvraag-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, inhoud MD5 = =, ETag =. |
| Microsoft.WindowsAzure.Storage |Waarschuwing |2 |85d077ab -… |Uitzondering geretourneerd tijdens de bewerking: De externe server heeft een fout geretourneerd: (403)-verboden... |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab -… |Controleren als de bewerking opnieuw moet worden uitgevoerd. Aantal nieuwe pogingen = 0, HTTP-statuscode 403, uitzondering = = van de externe server heeft een fout geretourneerd: (403)-verboden... |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab -… |De volgende locatie is ingesteld op de primaire, op basis van de locatiemodus. |
| Microsoft.WindowsAzure.Storage |Fout |1 |85d077ab -… |Beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Mislukt met de externe server heeft een fout geretourneerd: (403) Verboden. |

In dit scenario, moet u onderzoeken waarom het SAS-token is verlopen voordat de client het token naar de server stuurt:

* Normaal gesproken moet u een begintijd die bij het maken van een SAS voor een client direct kunt gebruiken niet instellen. Als er kleine klok verschillen tussen de host genereren van de SAS met behulp van de huidige tijd en de storage-service zijn, dan is het mogelijk voor de storage-service voor het ontvangen van een SA's dat is nog niet geldig.
* Moet u een zeer korte verlooptijd niet instellen op een SAS. Nogmaals, klok kleine verschillen tussen de host genereren van de SAS en de storage-service kunnen leiden tot een SAS blijkbaar verlopen ouder dan verwacht.
* Biedt de versie-parameter in de SAS-sleutel (bijvoorbeeld **sv = 2015-04-05**) overeenkomt met de versie van de Storage-clientbibliotheek die u gebruikt? Raden wij aan dat u altijd de nieuwste versie van de [Storage Client Library](https://www.nuget.org/packages/WindowsAzure.Storage/).
* Als u uw opslagtoegangssleutels opnieuw genereren, bestaande SAS-tokens mogelijk ongeldig worden gemaakt. Dit probleem kan zich voordoen als het genereren van SAS-tokens met een lange verlooptijd voor clienttoepassingen cache.

Als u de Opslagclientbibliotheek voor het genereren van SAS-tokens gebruikt, is het eenvoudig te maken van een geldig token. Echter, als u met behulp van de REST-API voor Storage en de SAS-tokens handmatig maken, Zie [toegang delegeren met een Shared Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>De client is ontvangen HTTP 404 (niet gevonden)-berichten
Als de clienttoepassing een HTTP 404 (niet gevonden)-bericht van de server ontvangt, betekent dit dat het object dat de client probeerde te gebruiken (zoals een entiteit, tabel, blob, container of wachtrij) niet in de storage-service bestaat. Er zijn een aantal mogelijke redenen hiervoor, zoals:

* [De client of een ander proces verwijderd het object eerder]
* [Een probleem met de autorisatie Shared Access Signature (SAS)]
* [Client-side JavaScript-code is niet gemachtigd voor toegang tot het object]
* [Netwerkfout]

#### <a name="client-previously-deleted-the-object"></a>De client of een ander proces verwijderd het object eerder
In scenario's waarin de client probeert te lezen, bijwerken of verwijderen van gegevens in een storage-service is het doorgaans gemakkelijk te herkennen in de logboeken van de server aan een eerdere bewerking dat het desbetreffende object verwijderd uit de storage-service. Vaak ziet de logboekgegevens u dat het object worden verwijderd door een andere gebruiker of een ander proces. In het logboek serverzijde logboekregistratie van opslag, het type bewerking en aangevraagd-object-sleutelkolommen laten zien wanneer een client een object verwijderd.

In het scenario waar een client is bij het invoegen van een object, deze mogelijk niet direct duidelijk waarom dit in een HTTP 404 (niet gevonden)-antwoord, resulteert gezien het feit dat de client is een nieuw object maken. Echter, als de client is het maken van een blob die deze moet kunnen vinden van de blob-container als de client is een bericht dat moet kunnen vinden van een wachtrij maakt, en als de client is het toevoegen van een rij deze moet kunnen vinden in de tabel.

U kunt het client-side-logboek van de Storage-clientbibliotheek gebruiken krijgen een meer gedetailleerde begrip van wanneer de client specifieke aanvragen naar de storage-service verzendt.

De volgende client-side-logboekbestanden die worden gegenereerd door de Storage-clientbibliotheek ziet u het probleem wanneer de client kan de container niet vinden voor de blob die wordt gemaakt. Dit logboek bevat details van de volgende opslagbewerkingen:

| Aanvraag-id | Bewerking |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** methode voor het verwijderen van de blob-container. Houd er rekening mee dat deze bewerking bevat een **HEAD** verzoek om te controleren of er sprake van de container. |
| e2d06d78… |**CreateIfNotExists** methode voor het maken van de blob-container. Houd er rekening mee dat deze bewerking bevat een **HEAD** aanvraag waarmee wordt gecontroleerd of de aanwezigheid van de container. De **HEAD** retourneert een 404-bericht, maar blijft. |
| de8b1c3c-... |**UploadFromStream** methode om de blob te maken. De **plaatsen** aanvraag is mislukt met een 404-bericht |

Logboekvermeldingen:

| Aanvraag-id | Bewerking tekst |
| --- | --- |
| 07b26a5d-... |Synchrone verzoek om te beginnen https://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| 07b26a5d-... |StringToSign = HEAD............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Wachten op antwoord. |
| 07b26a5d-... |Antwoord ontvangen. Statuscode 200, aanvraag-ID = = eeead849... Inhoud MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Antwoordheaders zijn voltooid, verwerkt u doorgaat met de rest van de bewerking. |
| 07b26a5d-... |Het downloaden van de hoofdtekst van antwoord. |
| 07b26a5d-... |De bewerking is voltooid. |
| 07b26a5d-... |Synchrone verzoek om te beginnen https://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| 07b26a5d-... |StringToSign = DELETE............x-ms-client-request-id:07b26a5d-....x-ms-date:Tue, 03 Jun 2014 10:33:12    GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Wachten op antwoord. |
| 07b26a5d-... |Antwoord ontvangen. Statuscode 202, aanvraag-ID = = 6ab2a4cf..., inhoud MD5 =, ETag =. |
| 07b26a5d-... |Antwoordheaders zijn voltooid, verwerkt u doorgaat met de rest van de bewerking. |
| 07b26a5d-... |Het downloaden van de hoofdtekst van antwoord. |
| 07b26a5d-... |De bewerking is voltooid. |
| e2d06d78-... |Asynchrone aanvraag om te beginnen https://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td> |
| e2d06d78-... |StringToSign = HEAD............x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Wachten op antwoord. |
| de8b1c3c-... |Synchrone verzoek om te beginnen https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |StringToSign = PUT... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-type:BlockBlob.x-MS-Client-Request-id:de8b1c3c-...x-MS-Date:TUE, 03 juni 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |Voorbereiden om aanvraaggegevens te schrijven. |
| e2d06d78-... |Uitzondering geretourneerd tijdens het wachten op reactie: De externe server heeft een fout geretourneerd: (404) niet gevonden... |
| e2d06d78-... |Antwoord ontvangen. Statuscode 404, aanvraag-ID = = 353ae3bc..., inhoud MD5 =, ETag =. |
| e2d06d78-... |Antwoordheaders zijn voltooid, verwerkt u doorgaat met de rest van de bewerking. |
| e2d06d78-... |Het downloaden van de hoofdtekst van antwoord. |
| e2d06d78-... |De bewerking is voltooid. |
| e2d06d78-... |Asynchrone aanvraag om te beginnen https://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| e2d06d78-... |StringToSign = PUT...0.........x-ms-client-request-id:e2d06d78-....x-ms-date:Tue, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Wachten op antwoord. |
| de8b1c3c-... |Gegevens van aanvragen van schrijven. |
| de8b1c3c-... |Wachten op antwoord. |
| e2d06d78-... |Uitzondering geretourneerd tijdens het wachten op reactie: De externe server heeft een fout geretourneerd: Conflict (409)... |
| e2d06d78-... |Antwoord ontvangen. Statuscode 409, aanvraag-ID = = c27da20e..., inhoud MD5 =, ETag =. |
| e2d06d78-... |Antwoordtekst voor fout bij het downloaden. |
| de8b1c3c-... |Uitzondering geretourneerd tijdens het wachten op reactie: De externe server heeft een fout geretourneerd: (404) niet gevonden... |
| de8b1c3c-... |Antwoord ontvangen. Statuscode 404, aanvraag-ID = = 0eaeab3e..., inhoud MD5 =, ETag =. |
| de8b1c3c-... |Uitzondering geretourneerd tijdens de bewerking: De externe server heeft een fout geretourneerd: (404) niet gevonden... |
| de8b1c3c-... |Beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Mislukt met de externe server heeft een fout geretourneerd: (404) niet gevonden... |
| e2d06d78-... |Beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Mislukt met de externe server heeft een fout geretourneerd: Conflict (409)... |

In dit voorbeeld toont het logboek dat de client is interleaving aanvragen van de **CreateIfNotExists** methode (aanvraag-ID e2d06d78...) met het aanvragen van de **UploadFromStream** -methode (de8b1c3c-...). Deze interleaving komt doordat de clienttoepassing is deze methoden asynchroon aanroepen. Wijzig de asynchrone code in de client om ervoor te zorgen dat de container wordt gemaakt voordat u probeert te uploaden van gegevens naar een blob in die container. In het ideale geval moet u alle uw containers van tevoren maken.

#### <a name="SAS-authorization-issue"></a>Een probleem met de autorisatie Shared Access Signature (SAS)
Als de clienttoepassing probeert te gebruiken van een SAS-sleutel die de benodigde machtigingen voor de bewerking niet bevat, retourneert de storage-service een HTTP 404 (niet gevonden)-bericht naar de client. Op hetzelfde moment, ziet u ook een andere waarde dan nul voor **SASAuthorizationError** in de metrische gegevens.

De volgende tabel ziet u een voorbeeld serverzijde log-bericht van het logboekbestand logboekregistratie van opslag:

| Name | Value |
| --- | --- |
| Begintijd van de aanvraag | 2014-05-30T06:17:48.4473697Z |
| Bewerkingstype     | GetBlobProperties            |
| Aanvraagstatus     | SASAuthorizationError        |
| HTTP-statuscode   | 404                          |
| Verificatietype| SAS                          |
| Servicetype       | Blob                         |
| Aanvraag-URL        | https://domemaildist.blob.core.windows.net/azureimblobcontainer/blobCreatedViaSAS.txt |
| &nbsp;                 |   ?sv=2014-02-14&sr=c&si=mypolicy&sig=XXXXX&;api-version=2014-02-14 |
| Aanvraag-ID-header  | a1f348d5-8032-4912-93ef-b393e5252a3b |
| Clientaanvraag-id  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


Onderzoek waarom de clienttoepassing wordt geprobeerd een bewerking waarvoor het is niet toegekend machtigingen uit te voeren.

#### <a name="JavaScript-code-does-not-have-permission"></a>JavaScript-code op de client heeft geen machtiging voor toegang tot het object
Als u van een JavaScript-client gebruikmaakt en de storage-service HTTP 404-berichten retourneert, controleert u de volgende JavaScript-fouten in de browser:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> U kunt de F12-hulpprogramma's voor ontwikkelaars in Internet Explorer gebruiken om de berichten uitgewisseld tussen de browser en de storage-service wanneer u het oplossen van problemen met client-side '-JavaScript.
> 
> 

Deze fouten optreden met de webbrowser implementeert de [beleid voor dezelfde oorsprong](http://www.w3.org/Security/wiki/Same_Origin_Policy) beveiligingsbeperking waarmee wordt voorkomen dat een webpagina aanroepen van een API in een ander domein uit het domein de pagina is afkomstig uit.

De JavaScript-probleem omzeilen, kunt u Cross-Origin Resource Sharing (CORS) configureren voor de storage-service die de client toegang heeft tot. Zie voor meer informatie, [Cross-Origin Resource Sharing (CORS) ondersteuning voor Azure Storage-Services](https://msdn.microsoft.com/library/azure/dn535601.aspx).

Het volgende codevoorbeeld laat zien hoe uw blobservice configureren voor het toestaan van JavaScript die worden uitgevoerd in de Contoso-domein voor toegang tot een blob in uw blob storage-service:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set the service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a>Netwerkfout
Verloren netwerkpakketten kunnen in sommige gevallen kan leiden tot de opslagservice retourneren van HTTP 404-berichten naar de client. Bijvoorbeeld, wanneer uw clienttoepassing een entiteit uit de tabelservice verwijderen is ziet u de client een opslagrapportage uitzondering genereert een ' HTTP 404 (niet gevonden) "statusbericht van de table-service. Wanneer u de tabel in de table storage-service onderzoeken, ziet u dat de service de entiteit niet verwijderen omdat aangevraagd.

De details van uitzondering in de client opnemen in de aanvraag-ID (7e84f12d...) door de table-service voor de aanvraag is toegewezen: u kunt deze informatie gebruiken om de details van de aanvraag niet vinden in de opslaglogboeken van de server-side-door te zoeken in de **aanvraag-id-header**  kolom in het logboekbestand. U kunt de metrische gegevens ook gebruiken om te identificeren als fouten, zoals dit zich voordoet en zoekt u in de logboekbestanden op basis van de tijd die de metrische gegevens over deze fout vastgelegd. Deze vermelding ziet u dat het verwijderen is mislukt met een statusbericht 'HTTP (404) Client andere fout'. De dezelfde logboekvermelding bevat ook de aanvraag-ID die is gegenereerd door de client in de **client-request-id** kolom (813ea74f...).

Het logboek serverzijde bevat ook een andere vermelding met dezelfde **client-request-id** waarde (813ea74f...) voor een geslaagde bewerking voor dezelfde entiteit en van dezelfde client verwijderen. Deze bewerking geslaagd heeft plaatsgevonden zeer kort voordat de mislukte aanvraag verwijderen.

De meest waarschijnlijke oorzaak van dit scenario is dat de client een delete-aanvraag voor de entiteit naar de tabelservice, die is voltooid verzonden, maar heeft geen een bevestiging ontvangen van de server (bijvoorbeeld vanwege een tijdelijk netwerkprobleem). De client vervolgens automatisch opnieuw geprobeerd de bewerking (met behulp van dezelfde **client-request-id**), en deze nieuwe poging is mislukt, omdat de entiteit al is verwijderd.

Als dit probleem regelmatig optreedt, moet u onderzoeken waarom de client is mislukt voor het ontvangen van bevestigingen van de table-service. Als het probleem onregelmatig is, moet u de fout 'HTTP (404) is niet gevonden' onderscheppen en het aanmelden bij de client, maar kan de client om door te gaan.

### <a name="the-client-is-receiving-409-messages"></a>De client ontvangt berichten van HTTP 409 (Conflict)
De volgende tabel ziet u een extraheren uit het logboek serverzijde voor twee clientbewerkingen: **DeleteIfExists** gevolgd door onmiddellijk **CreateIfNotExists** met behulp van de naam van de dezelfde blob-container. Elke clientbewerking resulteert in twee aanvragen verzonden naar de server eerst een **GetContainerProperties** verzoek om te controleren of de container bestaat, gevolgd door de **DeleteContainer** of  **CreateContainer** aanvraag.

| Tijdstempel | Bewerking | Resultaat | Containernaam | Clientaanvraag-id |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-… |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-… |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-… |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-… |

De code in de clienttoepassing wordt verwijderd en onmiddellijk opnieuw gemaakt een blob-container met dezelfde naam: de **CreateIfNotExists** methode (Client aanvraag ID bc881924-...) uiteindelijk mislukt met de fout HTTP 409 (Conflict). Wanneer een client wordt verwijderd blobcontainers, tabellen of wachtrijen die er een korte periode voordat u de naam is weer beschikbaar is.

De clienttoepassing moet unieke containernamen gebruiken wanneer er nieuwe containers wordt gemaakt als het patroon verwijderen/opnieuw gebruikelijk is.

### <a name="metrics-show-low-percent-success"></a>Prestatiegegevens geven lage PercentSuccess of vermeldingen in het analytics-bewerkingen met de status van ClientOtherErrors hebben
De **PercentSuccess** metriek bevat het percentage van bewerkingen die met succes zijn uitgevoerd op basis van de HTTP-statuscode. Bewerkingen met statuscodes van 2XX tellen als geslaagd, terwijl de bewerkingen met statuscodes in 3XX, 4XX en 5XX bereiken worden geteld als mislukt en lager de **PercentSucess** metrische waarde. In de logboekbestanden van de server-side-opslag, deze bewerkingen worden geregistreerd met de transactiestatus van de **ClientOtherErrors**.

Het is belangrijk te weten dat deze bewerkingen zijn voltooid en daarom niet van invloed op andere metrische gegevens zoals beschikbaarheid. Enkele voorbeelden van bewerkingen die is uitgevoerd, maar dat kan leiden tot mislukte HTTP-statuscodes zijn:

* **ResourceNotFound** (niet gevonden 404), bijvoorbeeld van een GET-aanvraag naar een blob die niet bestaat.
* **ResourceAlreadyExists** (Conflict 409), bijvoorbeeld van een **CreateIfNotExist** bewerking waar de resource al bestaat.
* **ConditionNotMet** (niet gewijzigd 304), bijvoorbeeld van een voorwaardelijke bewerking, zoals wanneer een client verzendt een **ETag** waarde en een HTTP- **If-None-Match** header om aan te vragen van een installatiekopie alleen als dat zo is bijgewerkt sinds de laatste bewerking.

U vindt een lijst met algemene REST-API-foutcodes die door de storage-services op de pagina [algemene foutcodes voor REST API](https://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Metrische gegevens over capaciteit geven een onverwachte toename in gebruik van de storage-capaciteit
Als u ziet een plotselinge, onverwachte wijzigingen in het capaciteitsverbruik in uw storage-account, kunt u onderzoeken de redenen door eerst op uw metrische gegevens over beschikbaarheid; bijvoorbeeld, een toename van het aantal mislukte aanvragen tot een toename van de hoeveelheid blob-opslag die u gebruikt leiden kunnen als u mogelijk hebt verwacht worden vrijmaken van ruimte vrij toepassingsspecifieke opschoonbewerkingen niet werkt zoals verwacht (bijvoorbeeld omdat de SAS-tokens die worden gebruikt voor het vrijmaken van ruimte zijn verlopen).

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Het probleem zich voordoet de opslagemulator gebruiken voor ontwikkeling of tests
U doorgaans de opslagemulator gebruiken tijdens het ontwikkelen en testen om te voorkomen dat de vereiste voor een Azure storage-account. De algemene problemen die optreden kunnen wanneer u de opslagemulator gebruikt, zijn:

* [Functie "X" werkt niet in de opslagemulator]
* [Fout 'de waarde voor een van de HTTP-headers is niet de juiste indeling' wanneer u de opslagemulator]
* [Het uitvoeren van de opslagemulator is vereist beheerdersmachtigingen]

#### <a name="feature-X-is-not-working"></a>Functie "X" werkt niet in de opslagemulator
De opslagemulator biedt geen ondersteuning voor alle functies van de Azure storage-services, zoals de file-service. Zie [Use the Azure Storage Emulator for Development and Testing](storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie.

Gebruik voor deze functies die de opslagemulator biedt geen ondersteuning voor de Azure storage-service in de cloud.

#### <a name="error-HTTP-header-not-correct-format"></a>Fout 'de waarde voor een van de HTTP-headers is niet de juiste indeling' wanneer u de opslagemulator
U test de toepassing die gebruikmaakt van de Storage Client Library op basis van de emulator en methode aanroepen voor lokale opslag, zoals **CreateIfNotExists** mislukt met het foutbericht 'de waarde voor een van de HTTP-headers is niet op de juiste indeling." Hiermee wordt aangegeven dat de versie van de opslagemulator die u gebruikt de versie van de storage-clientbibliotheek die u niet ondersteunt. De Opslagclientbibliotheek wordt toegevoegd de header **x-ms-version** op alle aanvragen die worden aangebracht. Als de opslagemulator niet herkend door de waarde in de **x-ms-version** header, wordt de aanvraag geweigerd.

U kunt de logboeken van de Bibliotheekclient opslag gebruiken om te zien van de waarde van de **x-ms-version-kopwaarde** deze verzendt. U ziet ook de waarde van de **x-ms-version-kopwaarde** als u Fiddler gebruikt voor het traceren van de aanvragen van uw clienttoepassing.

In dit scenario treedt meestal op als u installeert en de nieuwste versie van de Storage-clientbibliotheek gebruiken zonder dat de opslagemulator worden bijgewerkt. U moet installeren de nieuwste versie van de opslagemulator of opslag in de cloud gebruiken in plaats van de emulator voor ontwikkelen en testen.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Het uitvoeren van de opslagemulator is vereist beheerdersmachtigingen
U wordt gevraagd om beheerdersreferenties wanneer u de opslagemulator uitvoert. Dit gebeurt alleen als u bij het initialiseren van de opslagemulator voor de eerste keer. Nadat u hebt de opslagemulator geïnitialiseerd, hoeft u geen beheerdersrechten voor het opnieuw uitvoeren.

Zie [Use the Azure Storage Emulator for Development and Testing](storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie. U kunt ook de opslagemulator in Visual Studio, waardoor er ook beheerdersbevoegdheden initialiseren.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Ondervindt u problemen bij het installeren van de Azure SDK voor .NET
Wanneer u probeert de SDK te installeren, mislukt de bewerking probeert te installeren van de opslagemulator op uw lokale computer. Het installatielogboek bevat een van de volgende berichten:

* CAQuietExec:  Fout: Geen toegang tot SQL-exemplaar
* CAQuietExec:  Fout: Kan geen database maken

De oorzaak is een probleem met de bestaande installatie van LocalDB. De opslagemulator maakt standaard gebruik van LocalDB het behoud van gegevens wanneer deze overeenkomt met de Azure storage-services. U kunt de LocalDB-instantie opnieuw door te voeren van de volgende opdrachten in een opdrachtpromptvenster voordat u de SDK installeert.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

De **verwijderen** opdracht verwijdert u alle oude databasebestanden vanaf een vorige installatie van de opslagemulator.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>U hebt een ander probleem met een storage-service
Als de vorige secties voor het oplossen van problemen met het probleem dat u met een opslagservice ondervindt niet opgeeft, dient u de volgende benadering voor het opsporen en oplossen van uw probleem vast te stellen.

* Controleer uw metrische gegevens om te controleren of er een wijziging ten opzichte van de verwachte basislijn gedrag is. Uit de metrische gegevens, is het mogelijk om te bepalen of het probleem tijdelijk of permanent is en welke opslagbewerkingen het probleem invloed heeft op.
* U kunt de informatie van de metrische gegevens gebruiken om te zoeken naar uw server-side-logboekgegevens voor meer gedetailleerde informatie over eventuele fouten die optreden. Deze informatie kunt u het probleem kunt oplossen.
* Als de informatie in de server-side-Logboeken niet voldoende zijn is voor het oplossen van het probleem is, kunt u de logboeken van de client-side Storage-clientbibliotheek gebruiken voor het onderzoeken van het gedrag van uw clienttoepassing en hulpprogramma's zoals Fiddler, Wireshark en Microsoft Message Analyzer voor het onderzoeken van uw netwerk.

Zie voor meer informatie over het gebruik van Fiddler "[bijlage 1: Fiddler gebruiken om vast te leggen van HTTP en HTTPS-verkeer]. "

Zie voor meer informatie over het gebruik van Wireshark "[Bijlage 2: Met behulp van Wireshark om vast te leggen van netwerkverkeer]. "

Zie voor meer informatie over het gebruik van Microsoft Message Analyzer '[Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer]. "

## <a name="appendices"></a>Bijlagen
De bijlagen worden verschillende hulpprogramma's die mogelijk nuttig bij het vaststellen en oplossen van problemen met Azure Storage (en andere services) beschreven. Deze hulpprogramma's maken geen deel uit van Azure Storage, andere producten van derden zijn. Daarom de hulpprogramma's die worden beschreven in deze bijlagen zijn niet wordt gedekt door een eventuele ondersteuningsovereenkomst tot die u met Microsoft Azure of Azure Storage hebt, en daarom als onderdeel van uw evaluatieproces onderzoekt u de licenties en ondersteuning voor opties die beschikbaar zijn vanuit de providers van deze hulpprogramma's.

### <a name="appendix-1"></a>Bijlage 1: Fiddler gebruiken om vast te leggen van HTTP en HTTPS-verkeer
[Fiddler](http://www.telerik.com/fiddler) een handig hulpmiddel voor het analyseren van de HTTP en HTTPS-verkeer tussen uw clienttoepassing en de Azure storage-service die u gebruikt.

> [!NOTE]
> Fiddler kan worden gedecodeerd HTTPS-verkeer; Lees de documentatie van Fiddler zorgvuldig om te begrijpen hoe dit gebeurt en inzicht krijgen in de beveiligingsrisico's.
> 
> 

Deze bijlage bevat een kort overzicht van hoe u Fiddler om vast te leggen van verkeer tussen de lokale computer waarop u Fiddler hebt geïnstalleerd en de Azure storage-services configureert.

Wanneer u Fiddler hebt gestart, begint het vastleggen van HTTP en HTTPS-verkeer op uw lokale computer. Hier volgen enkele nuttige opdrachten voor het beheren van Fiddler:

* Stop en start het vastleggen van verkeer. In het hoofdmenu, gaat u naar **bestand** en klik vervolgens op **verkeer vastleggen** om uit te schakelen vast te leggen in of uit.
* Vastgelegde verkeersgegevens opslaan. In het hoofdmenu, gaat u naar **bestand**, klikt u op **opslaan**, en klik vervolgens op **alle sessies**: Hiermee kunt u het verkeer niet opslaan in een archief van de sessie-bestand. U kunt opnieuw laden van een sessie-archief later voor analyse, of als aangevraagd naar Microsoft ondersteuning verzenden.

Als u wilt beperken de hoeveelheid verkeer die Fiddler vastlegt, kunt u filters die u configureert in de **Filters** tabblad. De volgende schermafbeelding ziet u een filter dat wordt vastgelegd alleen verkeer dat wordt verzonden naar de **contosoemaildist.table.core.windows.net** opslageindpunt:

![][5]

### <a name="appendix-2"></a>Bijlage 2: Met behulp van Wireshark om vast te leggen van netwerkverkeer
[Wireshark](http://www.wireshark.org/) is een netwerkprotocolanalyse waarmee u om gedetailleerde pakketgegevens voor een breed scala aan netwerkprotocollen weer te geven.

De volgende procedure beschrijft hoe u om vast te leggen gedetailleerde pakketgegevens voor verkeer van de lokale computer waarop u Wireshark geïnstalleerd naar de tabelservice in uw Azure storage-account.

1. Start Wireshark op uw lokale computer.
2. In de **Start** sectie, selecteert u het lokale netwerk of interfaces die zijn verbonden met internet.
3. Klik op **vastleggen opties**.
4. Een filter toe te voegen de **vastleggen Filter** tekstvak. Bijvoorbeeld, **hosten contosoemaildist.table.core.windows.net** configureert Wireshark om vast te leggen alleen pakketten die zijn verzonden naar of van de service-eindpunt van de tabel in de **contosoemaildist** storage-account. Bekijk de [volledige lijst met Filters vastleggen](http://wiki.wireshark.org/CaptureFilters).
   
   ![][6]
5. Klik op **Start**. Wireshark nu vastlegt alle de pakketten verzenden naar of van het eindpunt van table service tijdens het gebruik van uw clienttoepassing op uw lokale computer.
6. Wanneer u klaar bent, op de Klik in het hoofdmenu **vastleggen** en vervolgens **stoppen**.
7. Als u wilt de vastgelegde gegevens opslaat in een bestand voor het vastleggen van Wireshark, klik in het hoofdmenu op **bestand** en vervolgens **opslaan**.

WireShark laten eventuele fouten die zijn opgenomen in de **packetlist** venster. U kunt ook de **deskundige informatie** venster (Klik op **analyseren**, vervolgens **deskundige informatie**) om een samenvatting van fouten en waarschuwingen weer te geven.

![][7]

U kunt er ook voor kiezen om weer te geven van de TCP-gegevens als het niveau van de toepassing ziet met de rechtermuisknop op de TCP-gegevens en selecteert u **Volg TCP Stream**. Dit is handig als u uw dump zonder een filter vastleggen vastgelegd. Zie voor meer informatie, [na de TCP-Streams](http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Zie voor meer informatie over het gebruik van Wireshark de [Wireshark gebruikershandleiding](http://www.wireshark.org/docs/wsug_html_chunked).
> 
> 

### <a name="appendix-3"></a>Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer
U kunt Microsoft Message Analyzer gebruiken om vast te leggen van HTTP en HTTPS-verkeer op een soortgelijke manier naar Fiddler en het netwerkverkeer op een soortgelijke manier te Wireshark opnemen.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Een web-traceringssessie met behulp van Microsoft Message Analyzer configureren
Het configureren van een web-traceringssessie voor HTTP en HTTPS-verkeer met behulp van Microsoft Message Analyzer, de toepassing Microsoft Message Analyzer wordt uitgevoerd en klik vervolgens op de **bestand** menu, klikt u op **vastleggen/Trace**. Selecteer in de lijst met beschikbare trace-scenario's, **webproxy**. Vervolgens in de **traceringsconfiguratie Scenario** deelvenster in de **HostnameFilter** tekstvak de namen van de eindpunten van uw opslag toevoegen (u kunt deze namen in opzoeken de [Azure-portal](https://portal.azure.com)). Bijvoorbeeld, als de naam van uw Azure storage-account is **contosodata**, u moet het volgende toevoegen aan de **HostnameFilter** tekstvak:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Een spatie scheidt de hostnamen.
> 
> 

Wanneer u bent klaar om te beginnen met het verzamelen van traceringsgegevens, klikt u op de **starten met** knop.

Voor meer informatie over de Microsoft Message Analyzer **webproxy** traceren, Zie [Provider van Microsoft-PEF-WebProxy](https://technet.microsoft.com/library/jj674814.aspx).

De ingebouwde **webproxy** tracering in Microsoft Message Analyzer is gebaseerd op Fiddler; deze kunt vastleggen van client-side-HTTPS-verkeer en niet-versleutelde HTTPS-berichten worden weergegeven. De **webproxy** werkt door het configureren van een lokale proxy voor alle HTTP en HTTPS-verkeer dat deze toegang tot niet-versleutelde berichten geeft traceren.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Met behulp van Microsoft Message Analyzer netwerkproblemen vaststellen
Naast het gebruik van de Microsoft Message Analyzer **webproxy** traceren om vast te leggen van de details van het HTTP/HTTPs-verkeer tussen de clienttoepassing en de storage-service, kunt u ook de ingebouwde **lokale Link-laag**  tracering voor het vastleggen van gegevens van de netwerk-pakket. Dit kunt u om vast te leggen die vergelijkbaar is met die welke u kunt vastleggen met Wireshark en diagnosticeren problemen zoals netwerk verloren gegane pakketten.

De volgende schermafbeelding ziet u een voorbeeld **lokale Link-laag** tracering met enkele **informatief** berichten in de **DiagnosisTypes** kolom. Te klikken op een pictogram in de **DiagnosisTypes** kolom ziet u de details van het bericht. In dit voorbeeld van de server opnieuw bericht #305 verzonden, omdat er geen een bevestiging van de client ontvangen:

![][9]

Wanneer u de traceersessie in Microsoft Message Analyzer maakt, kunt u filters reduceren het bedrag van de ruis in de tracering. Op de **vastleggen / traceren** pagina definieert u de tracering, klikt u op de **configureren** koppelen naast **Microsoft-Windows-NDIS-PacketCapture**. De volgende schermafbeelding ziet u een configuratie voor TCP-verkeer voor de IP-adressen van drie opslagservices filters:

![][10]

Zie voor meer informatie over de Microsoft Message Analyzer lokale koppeling Layer-tracering [Provider van Microsoft-PEF-NDIS-PacketCapture](https://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Bijlage 4: Met behulp van Excel weergeven van metrische gegevens en logboekgegevens
Veel hulpprogramma's kunnen u de metrische gegevens van Storage-gegevens downloaden uit Azure table storage in een opgemaakt met een scheidingsteken op waarmee u eenvoudig de gegevens laden in Excel voor het weergeven en analyseren. Storage-gegevens voor logboekregistratie van Azure blob-opslag is al in een door tekens gescheiden indeling die u in Excel laden kunt. U moet echter toevoegen juiste kolomkoppen op basis van de gegevens op in [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/azure/hh343259.aspx) en [tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx).

Uw opslag logboekregistratie om gegevens te importeren in Excel nadat u dit van de blob-opslag downloaden:

* Op de **gegevens** menu, klikt u op **van tekst**.
* Blader naar het logboekbestand dat u wilt weergeven en klik op **importeren**.
* In stap 1 van de **Wizard Tekst importeren**, selecteer **gescheiden**.

In stap 1 van de **Wizard Tekst importeren**, selecteer **puntkomma** als het enige scheidingsteken en kiest u dubbele-offerte als de **tekstscheidingsteken**. Klik vervolgens op **voltooien** en kies waar de gegevens in uw werkmap.

### <a name="appendix-5"></a>Bijlage 5: Bewaking met Application Insights voor Azure DevOps
U kunt ook de functie Application Insights voor Azure DevOps gebruiken als onderdeel van de beschikbaarheidsbewaking van prestaties en. Dit hulpprogramma kunt doen:

* Zorg ervoor dat uw web-service beschikbaar is en reageert. Of de app is een website of een apparaat-app die gebruikmaakt van een webservice, kan deze test uw URL om de paar minuten vanaf locaties over de hele wereld en laat u weten of er een probleem is.
* Analyseer snel eventuele problemen met prestaties of uitzonderingen in uw webservice. Bekijk als CPU- of andere bronnen worden uitgebreid, ontvang stack-traces van uitzonderingen en eenvoudig zoeken naar via logboektraceringen. Als de prestaties van de app onder aanvaardbare grenzen komt, Microsoft kunt u een e-mail verzenden. U kunt zowel .NET als Java web-services bewaken.

U vindt meer informatie op [wat is Application Insights](../../azure-monitor/app/app-insights-overview.md).

<!--Anchors-->
[Inleiding]: #introduction
[Hoe deze handleiding is georganiseerd]: #how-this-guide-is-organized

[Bewaking van uw storage-service]: #monitoring-your-storage-service
[Servicestatus controleren]: #monitoring-service-health
[Bewaking van capaciteit]: #monitoring-capacity
[Bewaken van de beschikbaarheid]: #monitoring-availability
[Prestaties bewaken]: #monitoring-performance

[Diagnose van opslagproblemen]: #diagnosing-storage-issues
[Problemen met de service controleren]: #service-health-issues
[Prestatieproblemen]: #performance-issues
[Diagnose van fouten]: #diagnosing-errors
[Opslagproblemen emulator]: #storage-emulator-issues
[Opslag Logboekregistratieprogramma 's]: #storage-logging-tools
[Met behulp van hulpprogramma's voor logboekregistratie]: #using-network-logging-tools

[End-to-end tracering]: #end-to-end-tracing
[Logboekgegevens correleren]: #correlating-log-data
[Clientaanvraag-ID]: #client-request-id
[Server aanvraag-ID]: #server-request-id
[Timestamps]: #timestamps

[Hulp bij het oplossen van problemen]: #troubleshooting-guidance
[Prestatiegegevens geven hoge AverageE2ELatency en lage AverageServerLatency aan]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Prestatiegegevens geven hoge AverageServerLatency aan]: #metrics-show-high-AverageServerLatency
[U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]: #you-are-experiencing-unexpected-delays-in-message-delivery

[Prestatiegegevens geven een toename in PercentThrottlingError aan]: #metrics-show-an-increase-in-PercentThrottlingError
[Tijdelijke toename in percentthrottlingerror aan]: #transient-increase-in-PercentThrottlingError
[Permanente toename in percentthrottlingerror aan fout]: #permanent-increase-in-PercentThrottlingError
[Prestatiegegevens geven een toename in PercentTimeoutError aan]: #metrics-show-an-increase-in-PercentTimeoutError
[Prestatiegegevens geven een toename in PercentNetworkError aan]: #metrics-show-an-increase-in-PercentNetworkError

[De client ontvangt HTTP 403-meldingen (verboden)]: #the-client-is-receiving-403-messages
[De client ontvangt HTTP 404-meldingen (niet gevonden)]: #the-client-is-receiving-404-messages
[De client of een ander proces verwijderd het object eerder]: #client-previously-deleted-the-object
[Een probleem met de autorisatie Shared Access Signature (SAS)]: #SAS-authorization-issue
[Client-side JavaScript-code is niet gemachtigd voor toegang tot het object]: #JavaScript-code-does-not-have-permission
[Netwerkfout]: #network-failure
[De client ontvangt berichten van de HTTP-409 (Conflict)]: #the-client-is-receiving-409-messages

[Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]: #metrics-show-low-percent-success
[Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]: #capacity-metrics-show-an-unexpected-increase
[Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]: #your-issue-arises-from-using-the-storage-emulator
[Functie "X" werkt niet in de opslagemulator]: #feature-X-is-not-working
[Fout 'de waarde voor een van de HTTP-headers is niet de juiste indeling' wanneer u de opslagemulator]: #error-HTTP-header-not-correct-format
[Het uitvoeren van de opslagemulator is vereist beheerdersmachtigingen]: #storage-emulator-requires-administrative-privileges
[U ondervindt problemen met het installeren van de Azure SDK voor .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[U hebt een ander probleem met storage-service]: #you-have-a-different-issue-with-a-storage-service

[Bijlagen]: #appendices
[Bijlage 1: Fiddler gebruiken om vast te leggen van HTTP en HTTPS-verkeer]: #appendix-1
[Bijlage 2: Met behulp van Wireshark om vast te leggen van netwerkverkeer]: #appendix-2
[Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer]: #appendix-3
[Bijlage 4: Met behulp van Excel weergeven van metrische gegevens en logboekgegevens]: #appendix-4
[Bijlage 5: Bewaking met Application Insights voor Azure DevOps]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
