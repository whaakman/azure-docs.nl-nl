---
title: Controleren, vaststellen en oplossen van Azure Storage | Microsoft Docs
description: Functies gebruiken zoals storage analytics, client-side '-logboekregistratie en andere hulpprogramma's van derden om te identificeren, opsporen en oplossen van problemen met Azure Storage met.
services: storage
documentationcenter: 
author: fhryo-msft
manager: jahogg
editor: tysonn
ms.assetid: d1e87d98-c763-4caa-ba20-2cf85f853303
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: fhryo-msft
ms.openlocfilehash: 1a9c9354b665294778886441cc6d7f02adb1163f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Microsoft Azure Storage bewaken, problemen opsporen en oplossen
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Overzicht
Diagnose van en het oplossen van problemen in een gedistribueerde toepassing die wordt gehost in een cloudomgeving kunnen worden complexer dan in traditionele omgevingen. Toepassingen kunnen worden geïmplementeerd in een PaaS of IaaS-infrastructuur on-premises, op een mobiel apparaat of in een combinatie hiervan. Normaal gesproken netwerkverkeer van uw toepassing lopen mogelijk langs openbare en particuliere netwerken en toepassingen kan gebruikmaken van meerdere opslagtechnologieën zoals Microsoft Azure Storage-tabellen, Blobs, wachtrijen of bestanden naast de andere gegevens zoals opgeslagen als relationele en documenteren van databases.

U moet deze proactief controleren en te begrijpen hoe vaststellen en oplossen van alle aspecten van deze en hun afhankelijke technologieën voor het beheren van dergelijke toepassingen is. Als een gebruiker van Azure Storage-services, moet u voortdurend controleren van de Storage-services die uw toepassing wordt gebruikt voor eventuele onverwachte wijzigingen in gedrag (zoals langzamer dan gebruikelijk reactietijden) en logboekregistratie gebruiken om meer gedetailleerde gegevens te verzamelen en analyseren van een probleem in de diepte. De diagnostische gegevens die u verkrijgen van zowel controle en logboekregistratie helpt u bij het vaststellen van de hoofdoorzaak van het probleem is opgetreden in uw toepassing. Vervolgens kunt u het probleem op te lossen en bepalen de juiste stappen die u ondernemen kunt worden opgelost. Azure Storage is een kern Azure-service en vormt een belangrijk onderdeel van het merendeel van de oplossingen die klanten in de Azure-infrastructuur implementeert. Azure Storage bevat mogelijkheden voor het vereenvoudigen van bewaking, onderzoeken en het oplossen van opslagproblemen in uw cloud-gebaseerde toepassingen.

> [!NOTE]
> De Azure-bestanden biedt geen ondersteuning voor logboekregistratie op dit moment.
> 

Zie voor een praktische handleiding voor het end-to-end het oplossen van problemen in toepassingen met Azure Storage [End-to-End problemen oplossen met metrische gegevens van Azure Storage en logboekregistratie, AzCopy en Message Analyzer](../storage-e2e-troubleshooting.md).

* [Inleiding]
  * [Rangschikking van deze handleiding]
* [bewaking van uw opslagservice]
  * [Bewaking servicestatus]
  * [Bewaking van capaciteit]
  * [Controleprogramma beschikbaarheid]
  * [Prestaties bewaken]
* [diagnose van opslagproblemen]
  * [Health service-problemen]
  * [Prestatieproblemen]
  * [Fouten opsporen]
  * [Emulator opslagproblemen]
  * [Logboekregistratieprogramma's voor opslag]
  * [Met behulp van hulpprogramma's voor logboekregistratie]
* [End-to-end tracering]
  * [Logboekgegevens correleren]
  * [Aanvraag-ID van client]
  * [Server aanvraag-ID]
  * [Tijdstempels]
* [richtlijnen voor probleemoplossing]
  * [metrische gegevens tonen AverageE2ELatency hoge en lage AverageServerLatency]
  * [Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]
  * [Prestatiegegevens geven hoge AverageServerLatency aan]
  * [U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]
  * [metrische gegevens tonen een toename in PercentThrottlingError]
  * [metrische gegevens tonen een toename in PercentTimeoutError]
  * [Prestatiegegevens geven een toename in PercentNetworkError aan]
  * [De client ontvangt berichten HTTP 403 (verboden)]
  * [De client ontvangt berichten HTTP 404 (niet gevonden)]
  * [De client ontvangt berichten van de HTTP-409 (Conflict)]
  * [metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]
  * [Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]
  * [Er is sprake van virtuele Machines met een groot aantal gekoppelde VHD's onverwacht opnieuw wordt opgestart]
  * [Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]
  * [U ondervindt problemen met het installeren van de Azure SDK voor .NET]
  * [U hebt een ander probleem met storage-service]
  * [Het oplossen van problemen met Windows Azure-bestanden](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Het oplossen van problemen met Linux Azure-bestanden](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [bijlagen]
  * [bijlage 1: HTTP en HTTPS-verkeer vastleggen met Fiddler]
  * [bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]
  * [bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]
  * [Bijlage 4: Met behulp van Excel metrische gegevens weergeven en gegevens aanmelden]
  * [Bijlage 5: Bewaking met Application Insights voor Visual Studio teamservices]

## <a name="introduction"></a>Inleiding
Deze handleiding wordt beschreven hoe u functies zoals Azure Storage Analytics clientzijde logboekregistratie in de Azure Storage-clientbibliotheek en andere hulpprogramma's van derden om te bepalen, onderzoeken en oplossen van Azure Storage problemen met betrekking tot.

![][1]

Deze handleiding is bedoeld om te lezen voornamelijk door ontwikkelaars van online services die gebruikmaken van Azure Storage-Services en IT-professionals die verantwoordelijk is voor het beheren van dergelijke online services. De doelstellingen van deze handleiding zijn:

* Om u te helpen houden de status en prestaties van uw Azure Storage-accounts.
* Om aan te bieden u de benodigde processen en hulpprogramma's om te bepalen of een probleem of een probleem in een toepassing is gekoppeld aan Azure Storage.
* Om u te bieden bruikbare richtlijnen voor het oplossen van problemen met betrekking tot Azure Storage.

### <a name="how-this-guide-is-organized"></a>Rangschikking van deze handleiding
De sectie '[bewaking van uw opslagservice]' wordt beschreven hoe u voor het bewaken van de status en prestaties van uw Azure Storage-services met Azure Storage Analytics metrische gegevens (metrische gegevens Storage).

De sectie '[diagnose van opslagproblemen]' wordt beschreven hoe u problemen met behulp van Azure Storage Analytics Logging (logboekregistratie van opslag) op te sporen. Tevens wordt beschreven hoe logboekregistratie voor client-side met behulp van de faciliteiten in een van de clientbibliotheken zoals de Storage-clientbibliotheek voor .NET of de Azure SDK voor Java inschakelen.

De sectie '[End-to-end tracering]' wordt beschreven hoe u de informatie in de verschillende logboekbestanden en metrische gegevens kunt correleren.

De sectie '[richtlijnen voor probleemoplossing]' biedt richtlijnen voor probleemoplossing voor sommige van de algemene opslag-gerelateerde problemen die u kunt tegenkomen.

De '[bijlagen]' bevatten informatie over het gebruik van andere hulpprogramma's zoals Wireshark en Netmon voor analyse van netwerk-pakketgegevens, Fiddler voor het analyseren van HTTP/HTTPS-berichten en Microsoft Message Analyzer voor het correleren van gegevens vastleggen.

## <a name="monitoring-your-storage-service"></a>Bewaking van uw storage-service
Als u bekend zijn met Windows performance monitoring bent, kunt u metrische gegevens Storage zien als wordt een Azure Storage-equivalent van de Prestatiemeter van Windows. In de opslag metrische gegevens vindt u een uitgebreide set met metrische gegevens (items in Prestatiemeter van Windows-terminologie) zoals servicebeschikbaarheid totaal aantal aanvragen voor de service of percentage van geslaagde aanvragen voor de service. Zie voor een volledige lijst van de beschikbare metrische gegevens [Storage Analytics metrische gegevens tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx). U kunt opgeven of u wilt dat de opslagservice te verzamelen en samenvoegen van metrische gegevens elk uur of elke minuut. Zie voor meer informatie over het inschakelen van metrische gegevens en bewaken van uw opslagaccounts [metrische gegevens storage inschakelen en weergeven van metrische gegevens](http://go.microsoft.com/fwlink/?LinkId=510865).

U kunt kiezen welke per uur metrische gegevens die u weergeven wilt de [Azure-portal](https://portal.azure.com) en regels configureren waarmee beheerders per e-mail wordt gewaarschuwd wanneer een per uur metrische gegevens een bepaalde drempelwaarde overschrijdt. Zie voor meer informatie [waarschuwingsmeldingen ontvangen](/azure/monitoring-and-diagnostics/monitoring-overview-alerts.md). 

De storage-service met een zo goed mogelijke poging metrische gegevens verzamelt, maar u kunt elke opslagbewerking niet registreren.

U kunt metrische gegevens zoals beschikbaarheid, totaal aantal aanvragen en gemiddelde latentie cijfers voor een opslagaccount weergeven in de Azure portal. Een meldingsregel is ook ingesteld zodat een beheerder wordt gewaarschuwd als beschikbaarheid onder een bepaalde mate zakt. Deze gegevens bekijken, een mogelijke gebied voor onderzoek Verwerkingsfrequentie van de tabel-service wordt dan 100% is (Zie voor meer informatie de sectie '[metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]').

Uw Azure-toepassingen om te controleren of dat ze zijn in orde en zoals verwacht door het uitvoeren, moet u continu controleren:

* Tot stand brengen van een basislijn metrische gegevens voor de toepassing waarmee u huidige gegevens vergelijken en identificeren van alle belangrijke wijzigingen in het gedrag van Azure storage en uw toepassing. De waarden van de metrische gegevens van uw basislijn, in veel gevallen worden specifieke toepassing en u moet tot stand worden gebracht wanneer u de prestaties van uw toepassing testen bent.
* Opname minuut metrische gegevens en het gebruik ervan controleren actief voor onverwachte fouten en afwijkingen zoals pieken in fout, telt of vraag tarieven.
* Opnemen per uur metrische gegevens en ze worden gebruikt voor het bewaken van gemiddelde waarden zoals gemiddelde aantallen van de fout en tarieven aanvragen.
* Onderzoeken van mogelijke problemen met diagnostische hulpprogramma's zoals verderop in de sectie '[diagnose van opslagproblemen]. "

De grafieken in de volgende afbeelding ziet u hoe de gemiddelde die voor de per uur metrische gegevens optreedt kunt verbergen pieken in de activiteit. De Uurlijkse metrische gegevens worden weergegeven om weer te geven van een constante snelheid van aanvragen, terwijl de minuut metrieken onthullen de schommelingen die echt plaatsvinden.

![][3]

De rest van deze sectie wordt beschreven welke metrische gegevens, moet u controleren en waarom.

### <a name="monitoring-service-health"></a>Bewaking servicestatus
U kunt de [Azure-portal](https://portal.azure.com) om de status van de Storage-service (en andere Azure-services) in alle Azure-regio's over de hele wereld. Hiermee kunt u onmiddellijk te zien als een probleem buiten uw invloedssfeer invloed op de Storage-service in de regio die u voor uw toepassing gebruiken.

De [Azure-portal](https://portal.azure.com) biedt ook meldingen van incidenten die invloed hebben op de verschillende Azure-services.
Opmerking: Deze informatie eerder beschikbaar was, samen met historische gegevens op de [servicedashboard Azure](http://status.azure.com).

Terwijl de [Azure-portal](https://portal.azure.com) verzamelt informatie uit in de Azure-datacenters (binnen-out bewaking), u kunt ook heeft een benadering buiten in voor het genereren van synthetische transacties die regelmatig toegang hebben tot uw Azure gehoste webtoepassing vanaf meerdere locaties. De services die worden aangeboden door [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) en Application Insights for Visual Studio Team Services zijn voorbeelden van deze benadering buiten in. Zie voor meer informatie over Application Insights voor Visual Studio Team Services, de bijlage '[bijlage 5: bewaking met Application Insights for Visual Studio teamservices](#appendix-5). "

### <a name="monitoring-capacity"></a>Bewaking van capaciteit
Metrische gegevens Storage alleen capaciteitsmetrieken voor de blob-service worden opgeslagen omdat blobs doorgaans voor het grootste deel van de opgeslagen gegevens account (op het moment van schrijven, het is niet mogelijk-opslag metrische gegevens gebruiken om te bewaken van de capaciteit van de tabellen en wachtrijen). U vindt deze gegevens in de **$MetricsCapacityBlob** tabel als u bewaking voor de Blob-service hebt ingeschakeld. Metrische gegevens Storage registreert deze gegevens eenmaal per dag en kunt u de waarde van de **RowKey** om te bepalen of de rij bevat een entiteit die is gekoppeld aan de gebruikersgegevens (waarde **gegevens**) of analytische gegevens (waarde **analytics**). Elke entiteit opgeslagen bevat informatie over de hoeveelheid opslagruimte gebruikt (**capaciteit** gemeten in bytes) en het huidige aantal containers (**ContainerCount**) en -blobs (**ObjectCount**) in het opslagaccount wordt gebruikt. Voor meer informatie over de capaciteit metrische gegevens opgeslagen in de **$MetricsCapacityBlob** tabel, Zie [Storage Analytics metrische gegevens tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Deze waarden voor een vroegtijdige waarschuwing dat u de capaciteitslimiet van uw opslagaccount nadert, moet u controleren. U kunt regels voor waarschuwingen om u te waarschuwen als cumulatieve opslaggebruik overschrijdt of minder is dan de drempels die u opgeeft toevoegen in de Azure portal.
> 
> 

Voor hulp bij het schatten van de grootte van verschillende opslagobjecten zoals blobs, Zie het blogbericht [wat Azure Storage facturering – bandbreedte, transacties en capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Controleprogramma beschikbaarheid
U moet de beschikbaarheid van de storage-services in uw opslagaccount bewaken door de bewaking van de waarde in de **beschikbaarheid** kolom in de tabellen per uur of minuut metrieken: **$MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$MetricsCapacityBlob**. De **beschikbaarheid** kolom bevat een percentagewaarde die aangeeft van de beschikbaarheid van de service of de API-bewerking dat wordt vertegenwoordigd door de rij (de **RowKey** bevat als de rij metrische gegevens voor de service als geheel of voor het uitvoeren van een specifieke API bevat).

Een waarde op die kleiner is dan 100% Hiermee wordt aangegeven dat bepaalde opslag-aanvragen mislukken. U kunt zien waarom ze zijn mislukt door de andere kolommen in de metrische gegevens die het aantal aanvragen met verschillende fouttypen zoals weergeven in **ServerTimeoutError**. U mag verwachten **beschikbaarheid** tijdelijk vallen onder de 100% om redenen als tijdelijke server time-outs tijdens de service wordt verplaatst partities naar betere taakverdeling aanvraag; de Pogingslogica in uw clienttoepassing die onregelmatige voorwaarden moet verwerken. Het artikel [Storage Analytics geregistreerde bewerkingen en statusberichten](http://msdn.microsoft.com/library/azure/hh343260.aspx) geeft een lijst van de transactietypen die metrische gegevens Storage bevat in de **beschikbaarheid** berekening.

In de [Azure-portal](https://portal.azure.com), kunt u regels voor waarschuwingen om u te waarschuwen als toevoegen **beschikbaarheid** voor een service valt onder de drempelwaarde die u opgeeft.

De '[richtlijnen voor probleemoplossing]' van deze handleiding worden enkele veelvoorkomende problemen beschreven voor opslag service betrekking hebben op beschikbaarheid.

### <a name="monitoring-performance"></a>Prestaties bewaken
U kunt de volgende metrische gegevens van de metrische gegevens per uur en minuut tabellen gebruiken voor het controleren van de prestaties van de storage-services.

* De waarden in de **AverageE2ELatency** en **AverageServerLatency** kolommen bevatten de gemiddelde tijd van de storage-service of type API-bewerking duurt naar aanvragen verwerkt. **AverageE2ELatency** is een meting van end-to-end-latentie die de tijd die nodig is omvat voor het lezen van de aanvraag en verzenden van het antwoord naast de tijd die het verzoek niet verwerken (dus inclusief netwerklatentie zodra de aanvraag de storage-service bereikt); **AverageServerLatency** is een meting van alleen de verwerkingstijd en daarom omvat niet alle netwerklatentie die betrekking hebben op de client communiceert. Zie de sectie '[metrische gegevens tonen AverageE2ELatency hoge en lage AverageServerLatency]' verderop in deze handleiding voor een beschrijving van waarom er mogelijk een aanzienlijk verschil tussen deze twee waarden.
* De waarden in de **TotalIngress** en **TotalEgress** kolommen bevatten de totale hoeveelheid gegevens, in bytes dat en zullen buiten uw storage-service of via een specifiek type van de API-bewerking.
* De waarden in de **TotalRequests** kolom tonen het totale aantal aanvragen dat is ontvangen van de storage-service van API-bewerking. **TotalRequests** is het totale aantal aanvragen dat de storage-service ontvangt.

Normaal gesproken bewaakt u onverwachte wijzigingen in een van deze waarden als een indicatie dat er een probleem waarvoor onderzoek vereist.

In de [Azure-portal](https://portal.azure.com), kunt u regels voor waarschuwingen om u te waarschuwen als een van de maatstaven voor prestaties voor deze service onder vallen of groter zijn dan een drempel die u opgeeft toevoegen.

De '[richtlijnen voor probleemoplossing]' van deze handleiding beschrijft een aantal algemene problemen met de opslag service betrekking hebben op prestaties.

## <a name="diagnosing-storage-issues"></a>Diagnose van opslagproblemen
Er zijn een aantal manieren dat u mogelijk op de hoogte van een probleem of een probleem in uw toepassing, deze omvatten:

* Een ernstige fout die ervoor zorgt de toepassing dat vastloopt of niet meer werkt.
* Belangrijke wijzigingen van basislijnwaarden in de metrische gegevens die u controleren wilt, zoals beschreven in de vorige sectie '[bewaking van uw opslagservice]. "
* Rapporten van gebruikers van uw toepassing die een bepaalde bewerking niet voltooid zoals verwacht of dat bepaalde functie niet werkt.
* Fouten die zijn gegenereerd binnen de toepassing die worden weergegeven in logboekbestanden of via een andere meldingsmethode.

Normaal gesproken problemen met Azure storage-services worden onderverdeeld in een van de vier hoofdcategorieën:

* Uw toepassing heeft een prestatieprobleem gerapporteerd door uw gebruikers of door wijzigingen in de maatstaven voor prestaties getoond.
* Er is een probleem met de Azure Storage-infrastructuur in een of meer regio's.
* Uw toepassing wordt uitgevoerd als een fout gerapporteerd door uw gebruikers of door een toename in een van de fout aantal metrische gegevens die u bewaakt getoond.
* Tijdens de ontwikkeling en tests u mogelijk gebruik van de emulator van de lokale opslag; enkele problemen beschreven die specifiek betrekking op informatie over het gebruik van de opslagemulator hebben kunnen optreden.

De volgende secties worden de stappen die u moet volgen voor het opsporen en oplossen van problemen in elk van de volgende vier categorieën. De sectie '[richtlijnen voor probleemoplossing]' verderop in deze handleiding biedt meer details voor enkele veelvoorkomende problemen kunnen optreden.

### <a name="service-health-issues"></a>Health service-problemen
Problemen met de status van de service zijn meestal buiten het besturingselement. De [Azure-portal](https://portal.azure.com) bevat informatie over actieve problemen met Azure-services met inbegrip van opslagservices. Als u hebt gekozen voor geografisch redundante opslag met leestoegang wanneer u uw opslagaccount hebt gemaakt, kan klikt u vervolgens in het geval van uw gegevens tijdelijk niet beschikbaar zijn op de primaire locatie, uw toepassing overschakelen tijdelijk in de alleen-lezen kopie op de secundaire locatie. Om dit te doen, moet uw toepassing kunnen schakelen tussen het gebruik van de primaire en secundaire opslaglocaties en kunnen werken in een modus met verminderde functionaliteit met alleen-lezen gegevens. De clientbibliotheken van Azure Storage kunnen u voor het definiëren van een beleid voor opnieuw proberen dat uit de secundaire opslag lezen kan als van de primaire opslag gelezen is mislukt. Uw toepassing moet ook Houd er rekening mee dat de gegevens in de secundaire locatie uiteindelijk consistent is. Zie voor meer informatie het blogbericht [opslagopties van Azure voor redundantie en geografisch redundante opslag met leestoegang](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Prestatieproblemen
De prestaties van een toepassing kunnen subjectief zijn, met name vanuit het perspectief van een gebruiker. Het is daarom belangrijk dat u over prestatiegegevens voor een basislijn beschikt aan de hand waarvan u kunt bepalen waar er prestatieproblemen zijn. Veel factoren kunnen invloed hebben op de prestaties van een Azure storage-service vanuit het perspectief van de toepassing client. Deze factoren kunnen werken in de storage-service, in de client of in de netwerkinfrastructuur; Daarom is het belangrijk dat u een strategie voor het identificeren van de oorsprong van het prestatieprobleem.

Nadat u de waarschijnlijk locatie van de oorzaak van het prestatieprobleem van de metrische gegevens hebt geïdentificeerd, kunt u de logboekbestanden vervolgens gebruiken om gedetailleerde informatie voor het opsporen en oplossen van het probleem verder te vinden.

De sectie '[richtlijnen voor probleemoplossing]' verderop in deze handleiding biedt meer informatie over algemene prestaties gerelateerd problemen die u kunt tegenkomen.

### <a name="diagnosing-errors"></a>Fouten opsporen
Gebruikers van uw toepassing kunnen melding van fouten die zijn gerapporteerd door de clienttoepassing. Metrische gegevens Storage registreert ook de aantallen voor verschillende fouttypen van uw storage-services zoals **NetworkError**, **ClientTimeoutError**, of **AuthorizationError**. Terwijl de opslag metrische gegevens worden alleen tellingen van verschillende fouttypen registreert, kunt u meer informatie over afzonderlijke aanvragen door te onderzoeken serverzijde en clientzijde netwerk Logboeken. De HTTP-statuscode geretourneerd door de storage-service krijgt doorgaans een indicatie van waarom de aanvraag is mislukt.

> [!NOTE]
> Houd er rekening mee dat u mag verwachten onregelmatige fouten: bijvoorbeeld door tijdelijke netwerkproblemen fouten of toepassingsfouten.
> 
> 

De volgende resources zijn nuttig voor de opslag-gerelateerde status en foutcodes begrijpen:

* [Algemene REST API-foutcodes](http://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Foutcodes voor BLOB-Service](http://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Foutcodes voor wachtrij](http://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Foutcodes voor tabel-Service](http://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Foutcodes voor bestand](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Emulator opslagproblemen
De Azure-SDK bevat een opslagemulator die u op een werkstation ontwikkeling uitvoeren kunt. Deze emulator simuleert de meeste van de werking van de Azure storage-services en is nuttig tijdens het ontwikkelen en testen, zodat u kunt toepassingen uitvoeren die gebruikmaken van Azure storage-services zonder de noodzaak van een Azure-abonnement en Azure storage-account.

De '[richtlijnen voor probleemoplossing]' van deze handleiding beschrijft enkele veelvoorkomende problemen aangetroffen met de opslagemulator.

### <a name="storage-logging-tools"></a>Logboekregistratieprogramma's voor opslag
Logboekregistratie van opslag biedt serverzijde registratie van aanvragen van de opslag in uw Azure storage-account. Zie voor meer informatie over het inschakelen van logboekregistratie voor serverzijde en toegang tot de logboekgegevens [opslag vastleggen inschakelen en toegang tot logboekgegevens](http://go.microsoft.com/fwlink/?LinkId=510867).

De Storage-clientbibliotheek voor .NET kunt u voor het verzamelen van client-side '-logboekgegevens die is gekoppeld aan opslagbewerkingen wordt uitgevoerd door uw toepassing. Zie voor meer informatie [clientzijde logboekregistratie met de Storage-clientbibliotheek voor .NET](http://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> In sommige gevallen (zoals SAS autorisatiefouten), kan een gebruiker waarvoor u geen aanvraaggegevens in de logboeken van de opslag serverzijde vinden kunt fout gemeld. U kunt de mogelijkheden voor logboekregistratie van het Storage-clientbibliotheek gebruiken voor het onderzoeken van als de oorzaak van het probleem op de client of -hulpprogramma's voor netwerkbewaking gebruikt voor het onderzoeken van het netwerk.
> 
> 

### <a name="using-network-logging-tools"></a>Met behulp van hulpprogramma's voor logboekregistratie
U kunt het verkeer tussen de client en server te bieden gedetailleerde informatie over de gegevens die de client en server uitwisselt en de onderliggende netwerkomstandigheden vastleggen. Nuttige hulpprogramma's voor logboekregistratie omvatten:

* [Fiddler](http://www.telerik.com/fiddler) is een gratis web proxy die u onderzoekt de kopteksten en de gegevens over de nettolading van HTTP en HTTPS-aanvraag en antwoord-berichten kunt opsporen. Zie voor meer informatie [bijlage 1: Fiddler om vast te leggen HTTP en HTTPS-verkeer met behulp van](#appendix-1).
* [Microsoft Network Monitor (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) en [Wireshark](http://www.wireshark.org/) zijn gratis netwerk protocol analyzers waarmee u kunt het gedetailleerde Pakketgegevens weergeven voor een breed scala aan-netwerkprotocollen. Zie voor meer informatie over Wireshark '[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer](#appendix-2)'.
* Microsoft Message Analyzer is een hulpprogramma van Microsoft die Netmon en die naast netwerkgegevens pakket vervangt, helpt u bij het weergeven en analyseren van de gegevens aan het logboek vastgelegd vanaf andere hulpprogramma's. Zie voor meer informatie '[bijlage 3: met behulp van Microsoft Message Analyzer om vast te leggen netwerkverkeer](#appendix-3)'.
* Als u een basisconnectiviteit test om te controleren wilt dat de clientcomputer verbinding met de Azure storage-service via het netwerk maken kan uitvoeren, u dit niet doen met de standaard **ping** hulpprogramma op de client. U kunt echter de [ **tcping** hulpprogramma](http://www.elifulkerson.com/projects/tcping.php) connectiviteit controleren.

In veel gevallen de logboekgegevens van registratie van opslag en de Storage-clientbibliotheek is voldoende voor het vaststellen van een probleem, maar in sommige scenario's, moet u mogelijk de meer gedetailleerde informatie die deze hulpprogramma's voor network logboekregistratie kunnen bieden. Bijvoorbeeld, kunt u header en de nettolading van gegevens die worden verzonden naar en van de storage-services, waarmee u om te onderzoeken hoe een clienttoepassing probeert opnieuw opslagbewerkingen weergeven met Fiddler om HTTP en HTTPS-berichten te bekijken. Protocol analyzers zoals Wireshark werken op pakketniveau waarmee u TCP-gegevens waarmee u problemen met verloren pakketten en verbindingsproblemen weergeven. Berichtanalyse kan werken op HTTP- en TCP-lagen.

## <a name="end-to-end-tracing"></a>End-to-end-tracering
End-to-end-tracering met een aantal logboekbestanden is een techniek nuttig voor het onderzoeken van mogelijke problemen. U kunt de datum/tijd-gegevens uit uw gegevens metrische gegevens gebruiken als indicatie van de locatie waar te bekijken in de logboekbestanden voor de gedetailleerde informatie waarmee u het probleem op te lossen.

### <a name="correlating-log-data"></a>Logboekgegevens correleren
Tijdens het weergeven van Logboeken van clienttoepassingen netwerk traceert en serverzijde opslag logboekregistratie, is het belangrijk om te correleren-aanvragen via de verschillende logboekbestanden. De logboekbestanden van opnemen een aantal verschillende velden die handig als de correlatie-id's zijn. De client-request-ID is het nuttigst veld gebruiken om te correleren vermeldingen in de verschillende logboeken. Maar in sommige gevallen, kan het handig zijn om de server aanvraag-ID of de tijdstempels te gebruiken. De volgende secties vindt u meer informatie over deze opties.

### <a name="client-request-id"></a>Aanvraag-ID van client
De Opslagclientbibliotheek genereert automatisch een unieke client aanvraag-ID voor elke aanvraag.

* In het logboek van clientzijde die de Storage-clientbibliotheek maakt de client-request-ID wordt weergegeven in de **aanvraag-ID van Client** veld van elke logboekvermelding met betrekking tot de aanvraag.
* In een netwerktracering zoals een vastgelegd door Fiddler, de client-request-ID is zichtbaar in aanvraagberichten als de **x-ms-client-request-id** waarde van de HTTP-header.
* In het logboek serverzijde opslag logboekregistratie wordt de client-request-ID weergegeven in de kolom-ID van Client-aanvraag.

> [!NOTE]
> Het is mogelijk voor meerdere aanvragen voor het delen van dezelfde client aanvraag-ID, omdat de client deze waarde worden toegewezen (Hoewel de Storage-clientbibliotheek automatisch een nieuwe waarde wijst). In het geval van nieuwe pogingen van de client alle pogingen dezelfde client aanvraag-id delen In het geval van een batch verzonden vanaf de client, heeft de batch een aanvraag één client-ID.
> 
> 

### <a name="server-request-id"></a>Aanvraag-ID van server
Server aanvraag-id's wordt automatisch gegenereerd door de storage-service.

* In het logboek serverzijde opslag logboekregistratie de aanvraag-ID wordt weergegeven de **aanvraag-ID header** kolom.
* In een netwerktracering zoals een vastgelegd door Fiddler, de aanvraag-ID wordt weergegeven in antwoordberichten als de **x-ms-aanvraag-id** waarde van de HTTP-header.
* In het logboek voor clientzijde die de Storage-clientbibliotheek maakt, wordt de aanvraag-ID weergegeven in de **bewerking tekst** kolom voor de details van de serverreactie met logboekvermelding.

> [!NOTE]
> De storage-service altijd een unieke server aanvraag-ID toewijst aan elke aanvraag die wordt ontvangen, zodat elke nieuwe poging van de client en elke bewerking die is opgenomen in een batch een unieke server-aanvraag-ID heeft.
> 
> 

Als de Storage-clientbibliotheek genereert een **StorageException** in de client de **RequestInformation** eigenschap bevat een **RequestResult** -object met een **ServiceRequestID** eigenschap. U kunt ook toegang tot een **RequestResult** object uit een **OperationContext** exemplaar.

Het volgende codevoorbeeld toont het instellen van een aangepaste **ClientRequestId** waarde door het koppelen van een **OperationContext** object van het verzoek om de storage-service. Ook ziet u hoe voor het ophalen van de **ServerRequestId** waarde van het antwoordbericht.

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
U kunt ook tijdstempels gerelateerde logboekvermeldingen vinden, maar wees voorzichtig met een tijdverschil tussen de client en server die zich kan voordoen. Plus of min 15 minuten voor de overeenkomende serverzijde vermeldingen op basis van de tijdstempel op de client moet worden gezocht. Houd er rekening mee dat de blobmetagegevens voor de blobs metrische gegevens met het tijdsbereik van de metrische gegevens opgeslagen in de blob; Dit is handig als u veel metrische gegevens blobs voor dezelfde minuut of uur.

## <a name="troubleshooting-guidance"></a>Richtlijnen voor probleemoplossing
Deze sectie helpt u bij de diagnose en het oplossen van enkele veelvoorkomende problemen van uw toepassing kan optreden wanneer u de Azure storage-services. Gebruik de onderstaande lijst om de informatie die relevant zijn voor uw specifieke probleem te vinden.

**Het oplossen van de beslissingsstructuur**

---
Uw probleem betrekking hebben op de prestaties van een van de storage-services?

* [metrische gegevens tonen AverageE2ELatency hoge en lage AverageServerLatency]
* [Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]
* [Prestatiegegevens geven hoge AverageServerLatency aan]
* [U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]

---
Uw probleem betrekking hebben op de beschikbaarheid van een van de storage-services?

* [metrische gegevens tonen een toename in PercentThrottlingError]
* [metrische gegevens tonen een toename in PercentTimeoutError]
* [Prestatiegegevens geven een toename in PercentNetworkError aan]

---
 De clienttoepassing ontvangt een HTTP-4XX (zoals 404) antwoord van een storage-service?

* [De client ontvangt berichten HTTP 403 (verboden)]
* [De client ontvangt berichten HTTP 404 (niet gevonden)]
* [De client ontvangt berichten van de HTTP-409 (Conflict)]

---
[metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]

---
[Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]

---
[Er is sprake van virtuele Machines met een groot aantal gekoppelde VHD's onverwacht opnieuw wordt opgestart]

---
[Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]

---
[U ondervindt problemen met het installeren van de Azure SDK voor .NET]

---
[U hebt een ander probleem met storage-service]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Metrische gegevens tonen AverageE2ELatency hoge en lage AverageServerLatency
De afbeelding hieronder uit de [Azure-portal](https://portal.azure.com) controlehulpprogramma toont een voorbeeld waarin de **AverageE2ELatency** aanzienlijk hoger is dan de **AverageServerLatency**.

![][4]

De opslagservice berekent alleen de metriek Opmerking **AverageE2ELatency** voor geslaagde aanvragen en, in tegenstelling tot **AverageServerLatency**, de duur van de client verzenden van de gegevens en bevestiging ontvangen van de storage-service bevat. Daarom een verschil tussen **AverageE2ELatency** en **AverageServerLatency** kan zijn vanwege de clienttoepassing wordt traag reageren of als gevolg van de voorwaarden op het netwerk.

> [!NOTE]
> U kunt ook weergeven **E2ELatency** en **ServerLatency** gegevens vastleggen voor afzonderlijke opslagbewerkingen in de logboekregistratie van opslag.
> 
> 

#### <a name="investigating-client-performance-issues"></a>Het onderzoeken van prestatieproblemen van client
Mogelijke oorzaken voor de client reageert traag zijn met een beperkt aantal beschikbare verbindingen of threads of te weinig bronnen zoals CPU, geheugen of netwerk bandbreedte wordt. U kunt mogelijk het probleem oplossen door het wijzigen van de clientcode voor het efficiënter zijn (bijvoorbeeld met behulp van asynchrone aanroepen van de storage-service) of met behulp van een grotere virtuele Machine (met meer kernen en meer geheugen).

Voor de tabel en wachtrij-services, het algoritme Nagle kan ook leiden tot hoge **AverageE2ELatency** in vergelijking met op **AverageServerLatency**: Zie voor meer informatie het bericht [van Nagle-algoritme is niet beschrijvende kleine aanvragen](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). U kunt het algoritme Nagle in code uitschakelen met behulp van de **ServicePointManager** -klasse in de **System.Net** naamruimte. U moet dit doen voordat u ervoor dat alle aanroepen naar de tabel of wachtrijservices in uw toepassing omdat dit geldt niet voor verbindingen die al zijn geopend. Het volgende voorbeeld is afkomstig uit de **Application_Start** methode in een werkrol.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Raadpleeg de logboeken van de clientzijde om te zien hoeveel aanvragen indienen van de clienttoepassing en controle voor algemene .NET gerelateerde knelpunten in uw client zoals CPU, .NET-garbagecollection, netwerkgebruik of geheugen. Als een beginpunt voor het oplossen van .NET-clienttoepassingen Zie [foutopsporing en tracering Profiling](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Netwerklatentieproblemen onderzoeken
Hoge end-to-end-latentie veroorzaakt door het netwerk is meestal vanwege tijdelijke omstandigheden. U kunt beide tijdelijke en permanente netwerkproblemen zoals verloren pakketten met hulpprogramma's zoals Wireshark of Microsoft Message Analyzer onderzoeken.

Zie voor meer informatie over het oplossen van problemen met netwerken met Wireshark '[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]. "

Zie voor meer informatie over het gebruik van Microsoft Message Analyzer problemen '[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]. "

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metrische gegevens tonen lage AverageE2ELatency en lage AverageServerLatency, maar de client ondervindt hoge latentie
In dit scenario is de meest waarschijnlijke oorzaak een vertraging in de opslag-aanvragen dat de opslagservice is bereikt. U moet onderzoeken waarom aanvragen van de client zijn niet waardoor het via voor de blob-service.

Een mogelijke reden voor de client verzenden van aanvragen vertragen is dat er een beperkt aantal beschikbare verbindingen of threads zijn.

Controleer ook of de client is bezig met meerdere pogingen en onderzoek de reden als dit het geval is. Om te bepalen of de client meerdere pogingen uitvoert, kunt u het volgende doen:

* Bekijk de logboekbestanden Storage Analytics. Als meerdere pogingen plaatsvinden, ziet u meerdere bewerkingen met dezelfde client aanvraag-ID, maar met verschillende serveraanvraag id's.
* Raadpleeg de clientlogboeken van de. Uitgebreide logboekregistratie wordt aangegeven dat een nieuwe poging is opgetreden.
* Fouten opsporen in uw code en controleer de eigenschappen van de **OperationContext** object dat is gekoppeld aan de aanvraag. Als de bewerking opnieuw is gestart, de **RequestResults** eigenschap meerdere unieke serveraanvraag id's bevat. U kunt ook de begin- en eindtijden voor elke aanvraag controleren. Zie voor meer informatie de voorbeeldcode in de sectie [Server aanvraag-ID].

Als er geen problemen in de client zijn, moet u onderzoeken van mogelijke netwerkproblemen zoals pakketverlies. U kunt hulpprogramma's zoals Wireshark of Microsoft Message Analyzer gebruiken voor het onderzoeken van problemen met netwerken.

Zie voor meer informatie over het oplossen van problemen met netwerken met Wireshark '[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]. "

Zie voor meer informatie over het gebruik van Microsoft Message Analyzer problemen '[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]. "

### <a name="metrics-show-high-AverageServerLatency"></a>Metrische gegevens tonen hoge AverageServerLatency
In het geval van hoog **AverageServerLatency** voor aanvragen voor het downloaden van blob, moet u de logboeken van de logboekregistratie voor opslag om te zien als er herhaalde aanvragen voor de blob dezelfde (of een reeks blobs). Voor blob uploaden aanvragen, moet u onderzoeken welke blok grootte van de client wordt gebruikt (bijvoorbeeld geblokkeerd minder dan 64 kB groot kan leiden tot overhead tenzij gelezen gegevens zich ook in minder dan 64 kB segmenten), en als meerdere clients blokken naar de dezelfde blob parallel uploadt. Controleer ook de per minuut metrische gegevens voor pieken in het aantal aanvragen weer die in meer dan resulteren de per tweede schaalbaarheidsdoelen: Zie ook '[metrische gegevens tonen een toename in PercentTimeoutError]. "

Als u hoog ziet **AverageServerLatency** blob gedownload aanvragen wanneer er worden herhaald aanvragen dezelfde blob of reeks blobs en vervolgens kunt u overwegen deze blobs met Azure-Cache of het Azure Content Delivery Network (CDN) opslaan in cache. Voor het uploaden van aanvragen, kunt u de doorvoer te verbeteren met behulp van een groter blok. Voor query's op tabellen is het ook mogelijk om te implementeren op clients die dezelfde querybewerkingen uitvoeren en waar de gegevens niet regelmatig worden gewijzigd caching aan clientzijde.

Hoge **AverageServerLatency** waarden kunnen ook worden veroorzaakt een symptoom van slecht geschreven tabellen of query's die leiden tot scanbewerkingen of die antivirusprogramma patroon append/toevoegen volgen. Zie '[metrische gegevens tonen een toename in PercentThrottlingError]' voor meer informatie.

> [!NOTE]
> U kunt een uitgebreide controlelijst voor prestaties controlelijst hier vinden: [Microsoft Azure Storage prestaties en schaalbaarheid controlelijst](storage-performance-checklist.md).
> 
> 

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>U ondervinden onverwachte vertragingen in de levering van berichten in een wachtrij
Als er een vertraging tussen het moment dat een toepassing wordt een bericht naar een wachtrij toegevoegd en de tijd dat deze beschikbaar om te lezen uit de wachtrij, moet u de volgende stappen uit om de oorzaak van het probleem te nemen:

* Controleer of dat de toepassing is met succes is met het toevoegen van de berichten naar de wachtrij. Controleer of de toepassing wordt niet opnieuw geprobeerd de **AddMessage** methode meerdere keren voordat slaagt. De logboeken van de Storage-clientbibliotheek worden alle herhaalde pogingen van opslagbewerkingen weergegeven.
* Controleer of er is geen klok scheeftrekken tussen de werkrol waarmee het bericht wordt toegevoegd aan de wachtrij en de werkrol die leest het bericht uit de wachtrij die het maakt worden weergegeven alsof er is een vertraging bij de verwerking.
* Controleer of de werkrol die de berichten kan uit de wachtrij lezen is niet mogelijk. Als u een wachtrij client roept de **GetMessage** methode, maar niet reageren met een bevestiging, het bericht onzichtbaar voor de wachtrij totdat blijft de **invisibilityTimeout** periode is verstreken. Het bericht wordt op dit moment beschikbaar voor het opnieuw verwerken.
* Controleer als de wachtrijlengte gedurende een bepaalde periode groeit. Dit kan gebeuren als er niet voldoende werknemers beschikbaar om te verwerken alle berichten die andere werknemers zijn die in de wachtrij. Controleer ook de metrische gegevens om te zien als delete-aanvragen mislukken en het aantal wachtrij halen op berichten, dit kan duiden op herhaalde mislukte pogingen tot het verwijderen van het bericht.
* Bekijk de logboekbestanden opslag logboekregistratie voor alle bewerkingen wachtrij die hoger dan verwacht **E2ELatency** en **ServerLatency** waarden gedurende een langere periode dan normaal.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Metrische gegevens tonen een toename in PercentThrottlingError
Bandbreedtebeperking fouten optreden wanneer u de schaalbaarheidsdoelen van een opslagservice overschrijdt. De opslagservice doet dit om ervoor te zorgen dat geen enkele client of de tenant de service ten koste van anderen kunt gebruiken. Zie voor meer informatie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie over schaalbaarheidsdoelen voor storage-accounts en prestatiedoelen voor partities binnen de storage-accounts.

Als de **PercentThrottlingError** metriek een toename in het percentage verzoeken die met een bandbreedteregeling fout mislukken zien, moet u voor het onderzoeken van een van twee scenario's:

* [Tijdelijke toename van PercentThrottlingError]
* [Permanente toename PercentThrottlingError fout]

Een toename van **PercentThrottlingError** vaak optreedt op hetzelfde moment als een toename van het aantal aanvragen voor opslag, of wanneer u in eerste instantie worden, laden uw toepassing testen. Dit kan ook zelf in de client als '503 Server bezet' of '500 time-out voor de bewerking' HTTP statusberichten opslagbewerkingen manifest.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Tijdelijke toename van PercentThrottlingError
Als u pieken in de waarde van ziet **PercentThrottlingError** die overeenkomen met perioden van hoge activiteit voor de toepassing, moet u een exponentiële (niet-lineair) terug uit een strategie voor nieuwe pogingen implementeren in uw client: Hiermee wordt de onmiddellijke belasting van de partitie verminderen en helpen uw toepassing vloeiend pieken in het verkeer. Zie voor meer informatie over het implementeren van beleid voor opnieuw proberen met behulp van de Storage-clientbibliotheek [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [!NOTE]
> U ziet misschien ook pieken in de waarde van **PercentThrottlingError** die niet samenvallen met perioden van hoge activiteit voor de toepassing: de meest waarschijnlijke oorzaak hier is de storage-service voor het verplaatsen van partities voor het verbeteren van taakverdeling.
> 
> 

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Permanente toename PercentThrottlingError fout
Als u ziet een consistent hoge waarde voor **PercentThrottlingError** na een permanente toename in de transactie-volumes, of als u uw eerste load uitvoert tests op uw toepassing, moet u om te evalueren hoe uw toepassing partities opslag gebruikt en of deze de schaalbaarheidsdoelen voor een opslagaccount bijna is bereikt. Bijvoorbeeld, als u fouten in een wachtrij (die telt als een enkele partitie) beperking ziet, moet klikt u vervolgens u extra wachtrijen gebruiken voor de transacties verdeeld over meerdere partities. Als u beperking van fouten in een tabel ziet, moet u overwegen om een andere partitieschema op uw transacties verdeeld over meerdere partities met behulp van een breed scala aan partitie sleutelwaarden. Een veelvoorkomende oorzaak van dit probleem is het prepend/append antivirusprogramma patroon waar u de datum selecteren als de partitiesleutel en vervolgens alle gegevens op een bepaalde dag wordt geschreven naar één partitie: belast, kan dit resulteren in een knelpunt schrijven. U moet overwegen van een ander partitionering ontwerp of evalueren of met behulp van blob-opslag is mogelijk een betere oplossing. U moet ook controleren of de beperking als gevolg van pieken in het verkeer optreedt en onderzoeken manieren van het vloeiend maken van het patroon van aanvragen.

Als u uw transacties over meerdere partities verdelen, moet u nog steeds op de hoogte van de limieten voor schaalbaarheid ingesteld voor het opslagaccount zijn. Bijvoorbeeld, als u tien wachtrijen elke het maximum van 2.000 1KB-berichten per seconde verwerken gebruikt, kunt u zich op de algehele limiet van 20.000 berichten per seconde voor het opslagaccount. Als u meer dan 20.000 entiteiten per seconde wordt verwerkt wilt, moet u overwegen meerdere opslagaccounts. U moet ook Houd er rekening mee dat de grootte van uw aanvragen en entiteiten invloed is op wanneer de clients in de storage-service worden beperkt: als u grotere aanvragen en entiteiten hebt, u sneller kan worden beperkt.

Inefficiënt Queryontwerp kan ook worden om de limieten voor schaalbaarheid voor Tabelpartities. Bijvoorbeeld, moet een query met een filter selecteert die alleen één procent van de entiteiten in een partitie, maar die scant alle entiteiten in een partitie toegang tot elke entiteit. Elke entiteit lezen meetelt voor het totale aantal transacties in de betreffende partitie; Daarom kunt u eenvoudig de van schaalbaarheidsdoelen bereiken.

> [!NOTE]
> De prestatietests, zou moeten uitwijzen queryontwerpen inefficiënt in uw toepassing.
> 
> 

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Metrische gegevens tonen een toename in PercentTimeoutError
De metrische gegevens tonen een toename van **PercentTimeoutError** voor een van uw storage-services. Op hetzelfde moment ontvangt de client een groot aantal statusberichten '500 time-out voor de bewerking' http-van-opslagbewerkingen.

> [!NOTE]
> Mogelijk ziet u time-outfouten tijdelijk als de storage-service aanvragen saldo's laden door een partitie te verplaatsen naar een nieuwe server.
> 
> 

De **PercentTimeoutError** metriek is een samenvoeging van de volgende metrische gegevens: **ClientTimeoutError**, **AnonymousClientTimeoutError**, **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**, en **SASServerTimeoutError**.

Time-outs voor de server worden veroorzaakt door een fout op de server. De clienttime-outs gebeuren omdat een bewerking op de server is groter dan de time-out die is opgegeven door de client. bijvoorbeeld, een client met de Storage-clientbibliotheek kunt instellen voor een time-out voor een bewerking via de **ServerTimeout** eigenschap van de **QueueRequestOptions** klasse.

Time-outs van de server wijzen op een probleem met de storage-service die verder onderzoek vereist. U kunt metrische gegevens gebruiken om te zien als u de limieten voor schaalbaarheid van de service zijn raken en om te identificeren van eventuele pieken in het verkeer dat dit probleem veroorzaakt. Als het probleem onderbroken wordt, kan zijn vanwege taakverdeling activiteit in de service. Als het probleem permanent is en niet wordt veroorzaakt door uw toepassing kunt u door de limieten voor schaalbaarheid van de service, moet u een probleem op te verhogen. Voor clienttime-outs, moet u beslissen als de time-out is ingesteld op een geschikte waarde in de client en een van de wijzigingen de time-outwaarde is ingesteld in de client of onderzoeken hoe kunt u de verbeteren de prestaties van de bewerkingen in de storage-service, bijvoorbeeld door het optimaliseren van uw tabel query's of de grootte van uw berichten beperken.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Metrische gegevens tonen een toename in PercentNetworkError
De metrische gegevens tonen een toename van **PercentNetworkError** voor een van uw storage-services. De **PercentNetworkError** metriek is een samenvoeging van de volgende metrische gegevens: **NetworkError**, **AnonymousNetworkError**, en **SASNetworkError**. Deze treden op wanneer de storage-service een netwerkfout detecteert wanneer de client een opslag-aanvraag indient.

De meest voorkomende oorzaak van deze fout is een client verbinding wordt verbroken voordat een time-out is verlopen in de storage-service. U moet de code onderzoeken in de client om te begrijpen waarom en wanneer de client wordt losgekoppeld van de storage-service. U kunt ook Wireshark, Microsoft Message Analyzer of Tcping gebruiken voor het onderzoeken van problemen met de netwerkverbinding van de client. Deze hulpprogramma's worden beschreven in de [bijlagen].

### <a name="the-client-is-receiving-403-messages"></a>De client ontvangt berichten HTTP 403 (verboden)
Als u de clienttoepassing die HTTP 403 (verboden) fouten, wordt er een waarschijnlijke oorzaak is dat de client een verlopen Shared Access Signature (SAS) wordt gebruikt wanneer het verzendt een aanvraag voor opslag (Hoewel andere mogelijke oorzaken klok scheeftrekken, ongeldige sleutels, en -koppen leeg zijn). Als dit wordt veroorzaakt door een verlopen SAS-sleutel, ziet u niet alle vermeldingen in de logboekgegevens van serverzijde opslag logboekregistratie. De volgende tabel ziet u een voorbeeld van het logboek voor clientzijde die worden gegenereerd door de Storage-clientbibliotheek die ziet u dit probleem optreedt:

| Bron | Uitgebreidheid | Uitgebreidheid | Aanvraag-id van client | Bewerking tekst |
| --- | --- | --- | --- | --- |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-... |Bewerking wordt gestart met de primaire locatie per locatie modus PrimaryOnly. |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-... |Synchrone aanvraag voor https://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14 starten&amp;sr = c&amp;si = mypolicy&amp;sig = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;api-version = 2014-02-14. |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-... |Wachten op reactie. |
| Microsoft.WindowsAzure.Storage |Waarschuwing |2 |85d077ab-... |Uitzondering geretourneerd tijdens het wachten op reactie: de externe server heeft een fout geretourneerd: (403) verboden... |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-... |Het antwoord is ontvangen. Statuscode = 403, aanvraag-ID = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, Content-MD5 =, ETag =. |
| Microsoft.WindowsAzure.Storage |Waarschuwing |2 |85d077ab-... |Uitzondering opgetreden tijdens de bewerking: de externe server heeft een fout geretourneerd: (403) verboden... |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-... |Controleren of de bewerking moet opnieuw worden geprobeerd. Aantal nieuwe pogingen = 0, HTTP-statuscode = 403, uitzondering = de externe server heeft een fout geretourneerd: (403) verboden... |
| Microsoft.WindowsAzure.Storage |Informatie |3 |85d077ab-... |De volgende locatie is ingesteld op de primaire, op basis van de locatie-modus. |
| Microsoft.WindowsAzure.Storage |Fout |1 |85d077ab-... |Beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Mislukt met de externe server heeft een fout geretourneerd: (403) verboden. |

In dit scenario moet u onderzoeken waarom het SAS-token is verlopen voordat de client het token naar de server stuurt:

* U moet een begintijd die bij het maken van een SAS voor een client direct kunt gebruiken doorgaans niet instellen. Als er klok kleine verschillen tussen de host genereren van de SAS met behulp van de huidige tijd en de storage-service, dan is het mogelijk voor de storage-service voor het ontvangen van een SAS die nog niet geldig.
* U moet een zeer korte verlooptijd niet instellen op een SAS. Opnieuw kunnen klok kleine verschillen tussen de host genereren van de SAS- en de storage-service leiden tot een SAS blijkbaar verloopt eerder dan verwacht.
* Biedt de versieparameter in de SAS-sleutel (bijvoorbeeld **AVP = 2015-04-05**) overeenkomt met de versie van de Storage-clientbibliotheek die u gebruikt? Het is raadzaam dat u altijd de nieuwste versie van gebruiken de [Storage-clientbibliotheek](https://www.nuget.org/packages/WindowsAzure.Storage/).
* Als u uw opslagtoegangssleutels opnieuw genereert, kan dit tot bestaande SAS-tokens ongeldig. Dit kan een probleem zijn als het genereren van SAS-tokens met een lange verlooptijdstip clienttoepassingen aan het cachegeheugen.

Als u de Storage-clientbibliotheek voor het genereren van SAS-tokens gebruikt, is het eenvoudig voor het bouwen van een geldig token. Echter, als u de REST-API voor Storage en handmatig maken van de SAS tokens zorgvuldig Lees het onderwerp [toegang delegeren met Shared Access Signature](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>De client ontvangt berichten HTTP 404 (niet gevonden)
Als de clienttoepassing een HTTP 404 (niet gevonden)-bericht van de server ontvangt, betekent dit dat het object dat de client probeerde te gebruiken (zoals een entiteit, tabel, blob, container of wachtrij) niet in de storage-service bestaat. Er zijn een aantal mogelijke redenen hiervoor, zoals:

* [De client of een ander proces eerder verwijderd van het object]
* [Een probleem met de autorisatie Shared Access Signature (SAS)]
* [Client-side JavaScript-code is niet gemachtigd voor toegang tot het object]
* [Netwerkfout]

#### <a name="client-previously-deleted-the-object"></a>De client of een ander proces eerder verwijderd van het object
In scenario's waar de client probeert te lezen, bijwerken of verwijderen van gegevens in een storage-service is het meestal gemakkelijk kunt herkennen in de logboeken van de server als een eerdere bewerking die het desbetreffende object verwijderd uit de storage-service. Heel vaak ziet de logboekgegevens u dat een andere gebruiker of proces het object verwijderd. In het logboek serverzijde opslag logboekregistratie het type van de bewerking en aangevraagd-object-sleutelkolommen ziet u wanneer een client een object verwijderd.

In het scenario waar een client is bij het invoegen van een object, deze mogelijk niet direct duidelijk waarom dit in een HTTP 404 (niet gevonden) antwoord resulteert gezien het feit dat de client een nieuw object maakt. Echter, als de client maakt een blob deze moet kunnen vinden van de blob-container, als de client is een bericht dat deze moet kunnen vinden van een wachtrij maakt, en als een rij is het toevoegen van de client deze moet kunnen worden gevonden in de tabel.

U kunt het client-side '-logboek van de Storage-clientbibliotheek krijgen een meer gedetailleerde begrip van wanneer de client specifieke aanvragen met de storage-service verzendt.

De volgende client-side logboek dat wordt gegenereerd door de Storage-clientbibliotheek ziet u het probleem wanneer de client kan de container voor de blob die het maakt niet vinden. Dit logboek bevat details van de volgende opslagbewerkingen:

| Aanvraag-id | Bewerking |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** methode om te verwijderen van de blob-container. Let op: deze bewerking bevat een **HEAD** aanvraag om te controleren op de aanwezigheid van de container. |
| e2d06d78... |**CreateIfNotExists** methode voor het maken van de blob-container. Let op: deze bewerking bevat een **HEAD** aanvraag waarmee wordt gecontroleerd of het bestaan van de container. De **HEAD** retourneert een 404-bericht, maar blijft. |
| de8b1c3c-... |**UploadFromStream** methode voor het maken van de blob. De **plaatsen** aanvraag is mislukt met een 404-bericht |

Logboekvermeldingen:

| Aanvraag-id | Bewerking tekst |
| --- | --- |
| 07b26a5d-... |Synchrone aanvraag voor https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart. |
| 07b26a5d-... |StringToSign HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 = 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Wachten op reactie. |
| 07b26a5d-... |Het antwoord is ontvangen. Statuscode 200, aanvraag-ID = = eeead849... Content-MD5 =, ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Antwoordheaders zijn verwerkt, u doorgaat met de rest van de bewerking. |
| 07b26a5d-... |Antwoordtekst downloaden. |
| 07b26a5d-... |De bewerking is voltooid. |
| 07b26a5d-... |Synchrone aanvraag voor https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart. |
| 07b26a5d-... |StringToSign DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 Jun 2014 = 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Wachten op reactie. |
| 07b26a5d-... |Het antwoord is ontvangen. Statuscode = 202, aanvraag-ID = 6ab2a4cf-..., Content-MD5 =, ETag =. |
| 07b26a5d-... |Antwoordheaders zijn verwerkt, u doorgaat met de rest van de bewerking. |
| 07b26a5d-... |Antwoordtekst downloaden. |
| 07b26a5d-... |De bewerking is voltooid. |
| e2d06d78-... |Asynchrone aanvraag voor https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart.</td> |
| e2d06d78-... |StringToSign HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 Jun 2014 = 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Wachten op reactie. |
| de8b1c3c-... |Synchrone aanvraag voor https://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt wordt gestart. |
| de8b1c3c-... |StringToSign PUT =... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-type:BlockBlob.x-MS-Client-Request-id:de8b1c3c-...x-MS-Date:TUE, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |Schrijven van gegevens van aanvragen wordt voorbereid. |
| e2d06d78-... |Uitzondering geretourneerd tijdens het wachten op reactie: de externe server heeft een fout geretourneerd: (404) niet gevonden... |
| e2d06d78-... |Het antwoord is ontvangen. Statuscode 404, aanvraag-ID = = 353ae3bc-..., Content-MD5 =, ETag =. |
| e2d06d78-... |Antwoordheaders zijn verwerkt, u doorgaat met de rest van de bewerking. |
| e2d06d78-... |Antwoordtekst downloaden. |
| e2d06d78-... |De bewerking is voltooid. |
| e2d06d78-... |Asynchrone aanvraag voor https://domemaildist.blob.core.windows.net/azuremmblobcontainer wordt gestart. |
| e2d06d78-... |StringToSign PUT =... 0...x-MS-Client-Request-id:e2d06d78-...x-MS-Date:TUE, 03 Jun 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Wachten op reactie. |
| de8b1c3c-... |Aanvraag voor het schrijven van gegevens. |
| de8b1c3c-... |Wachten op reactie. |
| e2d06d78-... |Uitzondering geretourneerd tijdens het wachten op reactie: de externe server heeft een fout geretourneerd: (409) Conflict... |
| e2d06d78-... |Het antwoord is ontvangen. Statuscode = 409, aanvraag-ID = c27da20e-..., Content-MD5 =, ETag =. |
| e2d06d78-... |Antwoordtekst voor fout bij het downloaden. |
| de8b1c3c-... |Uitzondering geretourneerd tijdens het wachten op reactie: de externe server heeft een fout geretourneerd: (404) niet gevonden... |
| de8b1c3c-... |Het antwoord is ontvangen. Statuscode 404, aanvraag-ID = = 0eaeab3e-..., Content-MD5 =, ETag =. |
| de8b1c3c-... |Uitzondering opgetreden tijdens de bewerking: de externe server heeft een fout geretourneerd: (404) niet gevonden... |
| de8b1c3c-... |Beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Mislukt met de externe server heeft een fout geretourneerd: (404) niet gevonden... |
| e2d06d78-... |Beleid voor opnieuw proberen is niet toegestaan voor een nieuwe poging. Mislukt met de externe server heeft een fout geretourneerd: (409) Conflict... |

In dit voorbeeld toont het logboek dat de client is interleaving aanvragen van de **CreateIfNotExists** methode (aanvraag-id e2d06d78...) met het aanvragen van de **UploadFromStream** methode (de8b1c3c-...); dit gebeurt omdat de clienttoepassing asynchroon deze methoden is aangeroepen. U kunt de asynchrone code in de client om ervoor te zorgen dat de container wordt gemaakt voordat u probeert te uploaden van gegevens naar een blob in de container moet wijzigen. In het ideale geval moet u alle uw containers van tevoren maken.

#### <a name="SAS-authorization-issue"></a>Een probleem met de autorisatie Shared Access Signature (SAS)
Als de clienttoepassing probeert te gebruiken van een SAS-sleutel die de benodigde machtigingen voor de bewerking niet bevat, retourneert de storage-service een HTTP 404 (niet gevonden)-bericht naar de client. Op hetzelfde moment ook ziet u een andere waarde dan nul voor **SASAuthorizationError** in de metrische gegevens.

De volgende tabel ziet u een voorbeeld serverzijde logboek-bericht van het logboekbestand opslag logboekregistratie:

| Naam | Waarde |
| --- | --- |
| Begintijd van de aanvraag | 2014-05-30T06:17:48.4473697Z |
| Bewerkingstype     | GetBlobProperties            |
| De status van aanvraag     | SASAuthorizationError        |
| HTTP-statuscode   | 404                          |
| Verificatietype| SAS                          |
| Servicetype       | Blob                         |
| Aanvraag-URL        | https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt |
| &nbsp;                 |   ? AVP 2014-02-14 = & sr = c & si = mypolicy & sig XXXXX =&;api-versie 2014-02-14 = |
| Koptekst van de aanvraag-id  | a1f348d5-8032-4912-93ef-b393e5252a3b |
| Aanvraag-ID van client  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


U moet onderzoeken waarom de clienttoepassing probeert een bewerking die er geen machtigingen heeft voor uit te voeren.

#### <a name="JavaScript-code-does-not-have-permission"></a>Client-side JavaScript-code is niet gemachtigd voor toegang tot het object
Als u van een JavaScript-client gebruikmaakt en de storage-service HTTP 404-berichten retourneert, controleert u de volgende JavaScript-fouten in de browser:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> U kunt de hulpprogramma's voor ontwikkelaars F12 in Internet Explorer gebruiken om te traceren, de berichten uitgewisseld tussen de browser en de storage-service als u de client-side JavaScript problemen wilt oplossen.
> 
> 

Deze fouten optreden omdat de webbrowser implementeert de [hetzelfde oorsprong beleid](http://www.w3.org/Security/wiki/Same_Origin_Policy) beveiligingsbeperkingen waarmee wordt voorkomen dat een webpagina een API aanroept in een ander domein uit het domein de pagina is afkomstig uit.

De JavaScript als probleem wilt verhelpen, kunt u Cross-Origin Resource delen (CORS) configureren voor de storage-service die is bij het openen van de client. Zie voor meer informatie [Cross-Origin-Resource delen (CORS) ondersteuning voor Azure Storage-Services](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Het volgende codevoorbeeld ziet u hoe uw blob-service zodat JavaScript uitgevoerd in het Contoso-domein toegang tot een blob in de blob storage-service te configureren:

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
In sommige gevallen verloren netwerkpakketten kunnen leiden tot de storage-service HTTP 404-berichten naar de client wordt geretourneerd. Bijvoorbeeld, als uw clienttoepassing van een entiteit uit de tabelservice verwijderen ziet u de client een opslagrapportage uitzondering genereert een ' HTTP 404 (niet gevonden) ' statusbericht uit de tabelservice. Als u de tabel in de tabel opslagservice onderzoeken, ziet u dat de service de entiteit heeft verwijderd, zoals aangevraagd.

De uitzonderingsdetails in de client opnemen in de aanvraag-id (7e84f12d...) die door de tabelservice voor de aanvraag is toegewezen: deze informatie kunt u de aanvraagdetails van de in de logboeken serverzijde opslag vinden door te zoeken in de **aanvraag-id-header** kolom in het logboekbestand. U kunt ook de metrische gegevens te identificeren wanneer fouten zoals deze optreden en zoek vervolgens de logboekbestanden op basis van de tijd die de metrische gegevens vastgelegd deze fout. Deze logboekvermelding ziet u dat het verwijderen is mislukt met een statusbericht 'HTTP (404) Client andere fout'. De dezelfde logboekvermelding bevat ook de aanvraag-id gegenereerd door de client in de **client-request-id** kolom (813ea74f...).

Het logboek serverzijde bevat ook een andere vermelding met dezelfde **client-request-id** waarde (813ea74f...) voor een geslaagde bewerking voor dezelfde entiteit en van dezelfde client verwijderen. Deze geslaagde bewerking heeft plaatsgevonden zeer snel voordat de mislukte aanvraag verwijderen.

De meest waarschijnlijke oorzaak van dit scenario is dat de client voor de entiteit een delete-aanvraag naar de tabelservice, die is voltooid verzonden, maar heeft geen een bevestiging ontvangen van de server maken (mogelijk vanwege een tijdelijk netwerkprobleem). De client vervolgens automatisch opnieuw geprobeerd de bewerking (met behulp van dezelfde **client-request-id**), en deze nieuwe poging is mislukt omdat de entiteit al is verwijderd.

Als dit probleem zich blijft voordoen, moet u onderzoeken waarom de client is niet ontvangen van bevestigingen uit de tabelservice. Als het probleem onderbroken wordt, moet u de fout 'HTTP (404) is niet gevonden' onderscheppen en het aanmelden bij de client, maar kan de client om door te gaan.

### <a name="the-client-is-receiving-409-messages"></a>De client ontvangt berichten van de HTTP-409 (Conflict)
De volgende tabel bevat een uitpakken uit het logboek serverzijde voor twee clientbewerkingen: **DeleteIfExists** gevolgd door onmiddellijk **CreateIfNotExists** met dezelfde naam van de blobcontainer. Opmerking dat elke clientbewerking in twee aanvragen verzonden naar de server eerst resulteert een **GetContainerProperties** aanvraag om te controleren of de container bestaat, gevolgd door de **DeleteContainer** of **CreateContainer** aanvraag.

| tijdstempel | Bewerking | Resultaat | Containernaam | Aanvraag-id van client |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-... |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-... |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-... |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-... |

De code in de clienttoepassing verwijderd en vervolgens onmiddellijk opnieuw een blob-container met dezelfde naam gemaakt: de **CreateIfNotExists** methode (Client aanvraag ID bc881924-...) uiteindelijk mislukt met de fout HTTP 409 (Conflict). Wanneer een client worden verwijderd blob-containers, tabellen of wachtrijen een korte voordat de naam periode weer beschikbaar.

De clienttoepassing moet unieke containernamen gebruiken wanneer er nieuwe containers maakt als het patroon verwijderen/opnieuw geldt.

### <a name="metrics-show-low-percent-success"></a>Metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors
De **PercentSuccess** metriek bevat het percentage van bewerkingen die met succes zijn uitgevoerd op basis van hun HTTP-statuscode. Bewerkingen met statuscodes van 2XX tellen als geslaagd, terwijl de bewerkingen met statuscodes in 3XX, 4XX en 5XX bereiken worden geteld als mislukt en lagere de **PercentSucess** metrische waarde. In de logboekbestanden serverzijde opslag deze bewerkingen worden geregistreerd met een transactiestatus **ClientOtherErrors**.

Het is belangrijk te weten dat deze bewerkingen zijn voltooid en daarom niet van invloed op andere metrische gegevens zoals beschikbaarheid. Enkele voorbeelden van bewerkingen die met succes uitvoeren, maar dat kan leiden tot mislukte HTTP-statuscodes zijn:

* **ResourceNotFound** (niet gevonden 404), bijvoorbeeld van een aanvraag voor ophalen naar een blob die niet bestaat.
* **ResouceAlreadyExists** (Conflict 409), bijvoorbeeld van een **CreateIfNotExist** bewerking waarbij de bron al bestaat.
* **ConditionNotMet** (niet gewijzigd 304), bijvoorbeeld van een voorwaardelijke bewerking zoals wanneer een client verzendt een **ETag** waarde en een HTTP- **If-None-Match** header voor het aanvragen van een installatiekopie alleen als deze is bijgewerkt sinds de laatste bewerking.

U vindt een lijst met algemene REST-API-foutcodes die de storage-services op de pagina retourneren [algemene foutcodes voor REST-API](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit
Als er plotseling, onverwachte wijzigingen in het capaciteitsverbruik in uw opslagaccount, kunt u onderzoeken de redenen door eerst te zoeken naar metrische gegevens over uw beschikbaarheid; bijvoorbeeld, een toename van het nummer van mislukte aanvragen tot een toename van het blob-opslag die u gebruikt leiden kunnen als u mogelijk hebt wilde worden vrijmaken van ruimte toepassingsspecifieke opschonen niet werkt zoals verwacht (bijvoorbeeld het verwijderen omdat de SAS-tokens die worden gebruikt voor het vrijmaken van ruimte verlopen).

### <a name="you-are-experiencing-unexpected-reboots"></a>U ondervindt onverwacht opnieuw wordt opgestart van Azure Virtual Machines waarvoor een groot aantal gekoppelde VHD 's
Als een Azure-virtuele Machine (VM) een groot aantal gekoppelde VHD's die zich in hetzelfde opslagaccount gebruikt heeft, kunt u de schaalbaarheidsdoelen voor een afzonderlijke opslagaccount, waardoor de virtuele machine mislukken kan overschrijdt. Controleer de minuut metrische gegevens voor het opslagaccount (**TotalRequests**/**TotalIngress**/**TotalEgress**) pieken die groter is dan de schaalbaarheidsdoelen voor een opslagaccount. Zie de sectie '[metrische gegevens tonen een toename in PercentThrottlingError]' voor hulp bij het bepalen of beperking van uw opslagaccount is opgetreden.

In het algemeen elke afzonderlijke invoer of uitvoerbewerking op een VHD van een virtuele Machine wordt omgezet in **downloaden pagina** of **pagina plaatsen** bewerkingen op de onderliggende paginablob. Daarom kunt u het geschatte aantal IOPS dat voor uw omgeving om af te stemmen hoeveel VHD's die u kunt hebben in een één opslagaccount op basis van het specifieke gedrag van uw toepassing. We raden niet meer dan 40 schijven in een één opslagaccount hebben. Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie over de huidige schaalbaarheidsdoelen voor storage-accounts, met name de totale aanvraag en de totale bandbreedte van het type opslagaccount u gebruikt.
Als u de van schaalbaarheidsdoelen voor uw opslagaccount overschreden, schakelt u uw VHD's in meerdere verschillende opslagaccounts te verminderen van de activiteit in elke rekening.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken
U doorgaans de opslagemulator gebruiken tijdens het ontwikkelen en testen om te voorkomen dat de vereiste voor een Azure storage-account. De veelvoorkomende problemen die optreden kunnen wanneer u de opslagemulator gebruikt, zijn:

* [De functie 'X' werkt niet in de opslagemulator]
* [Fout 'de waarde van een van de HTTP-headers heeft niet de juiste indeling' wanneer u de opslagemulator]
* [De opslagemulator moet beheerdersbevoegdheden]

#### <a name="feature-X-is-not-working"></a>De functie 'X' werkt niet in de opslagemulator
De opslagemulator ondersteunt niet alle functies van de Azure storage-services, zoals de file-service. Zie [Use the Azure Storage Emulator for Development and Testing](storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie.

Gebruik voor deze functies die de opslagemulator niet ondersteunt, de Azure storage-service in de cloud.

#### <a name="error-HTTP-header-not-correct-format"></a>Fout 'de waarde van een van de HTTP-headers heeft niet de juiste indeling' wanneer u de opslagemulator
U test de toepassing die gebruikmaakt van de clientbibliotheek van Storage op basis van de emulator en methode aanroepen van de lokale opslag, zoals **CreateIfNotExists** mislukt met het foutbericht ' de waarde voor een van de HTTP-headers heeft niet de juiste indeling." Hiermee wordt aangegeven dat de versie van de opslagemulator die u met de versie van de storage-clientbibliotheek die u niet ondersteunt. De Opslagclientbibliotheek Hiermee wordt de koptekst van **x-ms-version** op alle aanvragen die op deze manier. Als de opslagemulator worden niet herkend door de waarde in de **x-ms-version** -kop, wordt de aanvraag geweigerd.

U kunt de opslag Tapewisselaarclient-Logboeken kunt gebruiken om te zien van de waarde van de **header x-ms-version** is het verzenden. U ziet ook de waarde van de **header x-ms-version** als u Fiddler gebruiken om te traceren, de aanvragen van uw clienttoepassing.

Dit scenario treedt meestal op als u installeert en de nieuwste versie van de Storage-clientbibliotheek gebruiken zonder het bijwerken van de opslagemulator. U moet de meest recente versie van de opslagemulator installeert of cloud-opslag gebruiken in plaats van de emulator voor ontwikkeling en tests.

#### <a name="storage-emulator-requires-administrative-privileges"></a>De opslagemulator moet beheerdersbevoegdheden
U wordt gevraagd om beheerdersreferenties wanneer u de opslagemulator uitvoert. Dit gebeurt alleen als u bij het initialiseren van de opslagemulator voor de eerste keer. Nadat u hebt de opslagemulator geïnitialiseerd, hoeft u geen beheerdersbevoegdheden opnieuw uitvoeren.

Zie [Use the Azure Storage Emulator for Development and Testing](storage-use-emulator.md) (De Azure-opslagemulator gebruiken voor het ontwikkelen en testen) voor meer informatie. Houd er rekening mee dat u ook de opslagemulator in Visual Studio, wordt ook hebt u beheerdersbevoegdheden nodig kan worden geïnitialiseerd.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>U ondervindt problemen met het installeren van de Azure SDK voor .NET
Wanneer u de SDK installeren, mislukt dit probeert te installeren van de opslagemulator op uw lokale machine. Het installatielogboek bevat een van de volgende berichten:

* CAQuietExec: Fout: geen toegang tot SQL-exemplaar
* CAQuietExec: Fout: kan geen database maken

De oorzaak is een probleem met een bestaande installatie van de LocalDB. De opslagemulator maakt standaard gebruik van LocalDB om gegevens als deze overeenkomt met de Azure storage-services. U kunt uw LocalDB-exemplaar herstellen met de volgende opdrachten in een opdrachtpromptvenster voordat u de SDK installeren.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

De **verwijderen** opdracht wordt een oude databasebestanden uit eerdere installaties van de opslagemulator verwijderd.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>U hebt een ander probleem met storage-service
Als de vorige secties het probleem met de storage-service niet opgeeft, dient u de volgende benadering voor de diagnose en het oplossen van uw probleem vast te stellen.

* Controleer de metrische gegevens om te controleren of er elke wijziging van de verwachte basislijn gedrag wordt. Van de metrische gegevens, is het mogelijk om te bepalen of het probleem tijdelijke of permanente is en welke opslagbewerkingen een invloed op het probleem.
* U kunt de metrische gegevens voor hulp bij het zoeken van uw server-side '-logboekgegevens voor meer gedetailleerde informatie over eventuele fouten die optreden. Deze informatie kunt u het probleem kunt oplossen.
* Als de informatie in de logboeken serverzijde niet voldoende zijn is voor het oplossen van het probleem is, kunt u de logboeken van de client-side Storage-clientbibliotheek gebruiken voor het onderzoeken van het gedrag van uw clienttoepassing en hulpprogramma's zoals Fiddler, Wireshark en Microsoft Message Analyzer voor het onderzoeken van uw netwerk.

Zie voor meer informatie over het gebruik van Fiddler '[bijlage 1: HTTP en HTTPS-verkeer vastleggen met Fiddler]. "

Zie voor meer informatie over het gebruik van Wireshark '[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]. "

Zie voor meer informatie over het gebruik van Microsoft Message Analyzer '[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]. "

## <a name="appendices"></a>Bijlagen
In de bijlagen beschrijven verschillende hulpprogramma's die mogelijk nuttig bij het opsporen en het oplossen van problemen met Azure Storage (en andere services). Deze hulpprogramma's maken geen deel uit van Azure Storage en andere producten van derden zijn. Als zodanig worden de hulpprogramma's beschreven in deze bijlagen niet wordt gedekt door een ondersteuningsovereenkomst mogelijk met Microsoft Azure of Azure Storage en daarom als onderdeel van uw evaluatieproces onderzoekt u de licentie- en ondersteuningsbeleid beschikbare opties van de providers van deze hulpprogramma's.

### <a name="appendix-1"></a>Bijlage 1: Gebruik Fiddler om vast te leggen HTTP en HTTPS-verkeer
[Fiddler](http://www.telerik.com/fiddler) een handig hulpmiddel voor het analyseren van de HTTP en HTTPS-verkeer tussen de clienttoepassing en de Azure storage-service die u gebruikt.

> [!NOTE]
> Fiddler decodering HTTPS-verkeer; Lees de documentatie Fiddler zorgvuldig om te begrijpen hoe dit wordt uitgevoerd en inzicht krijgen in de beveiligingsrisico's.
> 
> 

Deze bijlage bevat een kort overzicht van hoe u Fiddler om vast te leggen van verkeer tussen de lokale computer waarop u Fiddler hebt geïnstalleerd en de Azure storage-services configureert.

Wanneer u Fiddler hebt gestart, begint het vastleggen van HTTP en HTTPS-verkeer op uw lokale machine. Hier volgen enkele nuttige opdrachten voor het beheren van Fiddler:

* Stop en start het vastleggen van verkeer. In het hoofdmenu, gaat u naar **bestand** en klik vervolgens op **verkeer vastleggen** in-of uitschakelen vastleggen in- of uitschakelen.
* Van de vastgelegde verkeersgegevens opslaan. In het hoofdmenu, gaat u naar **bestand**, klikt u op **opslaan**, en klik vervolgens op **alle sessies**: Hiermee kunt u het verkeer niet opslaan in een sessie archiefbestand. U kunt een sessie-archief voor analyse later opnieuw laden of verzenden als aangevraagd aan Microsoft ondersteuning.

Als u wilt beperken de hoeveelheid verkeer die Fiddler worden vastgelegd, kunt u filters die u configureert in de **Filters** tabblad. De volgende schermafbeelding ziet u een filter dat wordt vastgelegd alleen verkeer dat wordt verzonden naar de **contosoemaildist.table.core.windows.net** opslag eindpunt:

![][5]

### <a name="appendix-2"></a>Bijlage 2: Wireshark gebruiken voor het vastleggen van netwerkverkeer
[Wireshark](http://www.wireshark.org/) is een netwerkprotocolanalyse waarmee u kunt het gedetailleerde Pakketgegevens weergeven voor een breed scala aan-netwerkprotocollen.

De volgende procedure ziet u hoe om vast te leggen gedetailleerde pakketgegevens voor verkeer van de lokale computer waarop u Wireshark geïnstalleerd aan de tabelservice in uw Azure storage-account.

1. Start Wireshark op uw lokale machine.
2. In de **Start** sectie, selecteert u het lokale netwerk of interfaces die zijn verbonden met internet.
3. Klik op **vastleggen opties**.
4. Een filter toe te voegen de **vastleggen Filter** textbox. Bijvoorbeeld: **hosten contosoemaildist.table.core.windows.net** configureert Wireshark om vast te leggen alleen pakketten die worden verzonden naar of van het service-eindpunt van de tabel in de **contosoemaildist** storage-account. Bekijk de [volledige lijst met Filters vastleggen](http://wiki.wireshark.org/CaptureFilters).
   
   ![][6]
5. Klik op **Start**. Wireshark zal nu alle de pakketten verzenden naar of van het eindpunt tabel-service tijdens het gebruik van uw client-toepassing op uw lokale machine vastgelegd.
6. Wanneer u klaar bent, in het hoofdmenu klikt u op **vastleggen** en vervolgens **stoppen**.
7. Als u wilt de vastgelegde gegevens opslaan in een bestand voor het vastleggen van Wireshark, klik op het hoofdmenu op **bestand** en vervolgens **opslaan**.

WireShark wordt gemarkeerd eventuele fouten die zijn opgenomen in de **packetlist** venster. Bovendien kunt u de **Expert Info** venster (Klik op **analyseren**, vervolgens **Expert Info**) om een samenvatting van fouten en waarschuwingen weer te geven.

![][7]

U kunt er ook voor kiezen om weer te geven van de TCP-gegevens zoals de toepassingslaag dit ziet door met de rechtermuisknop op de TCP-gegevens en te selecteren **TCP-stroom Volg**. Dit is vooral handig als u uw dump zonder een filter vastleggen zijn vastgelegd. Zie voor meer informatie [volgende TCP-stromen](http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Zie voor meer informatie over het gebruik van Wireshark de [Wireshark gebruikershandleiding](http://www.wireshark.org/docs/wsug_html_chunked).
> 
> 

### <a name="appendix-3"></a>Bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen van netwerkverkeer
U kunt Microsoft Message Analyzer gebruiken om vast te leggen van HTTP en HTTPS-verkeer op vergelijkbare wijze als bij Fiddler en netwerkverkeer vastleggen in een vergelijkbare manier als bij Wireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Een websessie tracering met behulp van Microsoft Message Analyzer configureren
Voor het configureren van een web-traceringssessie HTTP en HTTPS-verkeer met behulp van Microsoft Message Analyzer, voert u de toepassing Microsoft Message Analyzer en klik vervolgens op de **bestand** menu, klikt u op **vastleggen/Trace**. Selecteer in de lijst van beschikbare trace-scenario's **webproxy**. Vervolgens in de **traceringsconfiguratie Scenario** paneel in de **HostnameFilter** textbox de namen van de eindpunten van uw opslag toevoegen (u kunt deze namen opzoeken in de [Azure-portal](https://portal.azure.com)). Als de naam van uw Azure storage-account is bijvoorbeeld **contosodata**, u moet het volgende toevoegen aan de **HostnameFilter** textbox:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> De hostnamen wordt gescheiden door een spatie.
> 
> 

Wanneer u klaar bent voor begint met het verzamelen van traceringsgegevens, klikt u op de **starten met** knop.

Voor meer informatie over de Microsoft Message Analyzer **webproxy** traceren, Zie [Provider van Microsoft-PEF-WebProxy](http://technet.microsoft.com/library/jj674814.aspx).

De ingebouwde **webproxy** tracering in Microsoft Message Analyzer is gebaseerd op Fiddler; het kan vastleggen clientzijde HTTPS-verkeer en niet-versleutelde HTTPS-berichten worden weergegeven. De **webproxy** werkt door het configureren van een lokale proxy voor alle HTTP en HTTPS-verkeer dat deze toegang tot niet-versleutelde berichten geeft traceren.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnose van netwerkproblemen met behulp van Microsoft Message Analyzer
Naast het gebruik van de Microsoft Message Analyzer **webproxy** traceren om vast te leggen van de details van het HTTP/HTTPs-verkeer tussen de clienttoepassing en de storage-service, kunt u ook de ingebouwde **lokale Link-laag** tracering voor het vastleggen van gegevens van de netwerk-pakket. Dit kunt u voor het vastleggen van gegevens is vergelijkbaar met die welke u kunt vastleggen met Wireshark en diagnose problemen zoals netwerk verloren pakketten.

De volgende schermafbeelding ziet u een voorbeeld **lokale Link-laag** traceren met een aantal **informatief** berichten in de **DiagnosisTypes** kolom. Te klikken op een pictogram in de **DiagnosisTypes** kolom ziet u de details van het bericht. In dit voorbeeld van de server opnieuw bericht #305 verzonden, omdat er geen bevestiging is ontvangen van de client:

![][9]

Wanneer u de traceersessie in Microsoft Message Analyzer maakt, kunt u filters Verklein de hoeveelheid ruis in de tracering. Op de **vastleggen / traceren** pagina waar u de tracering definiëren, klikt u op de **configureren** koppelen naast **Microsoft Windows-NDIS PacketCapture**. De volgende Schermafbeelding toont een configuratie waarmee TCP-verkeer voor de IP-adressen van drie opslagservices gefilterd:

![][10]

Zie voor meer informatie over de tracering van Microsoft Message Analyzer lokale Link-laag, [Provider van Microsoft-PEF-NDIS-PacketCapture](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Bijlage 4: Met behulp van Excel metrische gegevens weergeven en gegevens aanmelden
Veel hulpprogramma's kunnen u downloaden van de metrische gegevens Storage-gegevens uit Azure-tabelopslag gescheiden indeling kunt u gemakkelijk de gegevens in Excel laden voor weergave en analyse. Logboekgegevens van de opslag van Azure blob-opslag is al een gescheiden indeling die u in Excel laden kunt. U moet echter toevoegen juiste kolomkoppen op basis van de informatie op [Storage Analytics logboekindeling](http://msdn.microsoft.com/library/azure/hh343259.aspx) en [Storage Analytics metrische gegevens tabelschema](http://msdn.microsoft.com/library/azure/hh343264.aspx).

Uw opslag logboekregistratie om gegevens te importeren in Excel nadat u dit van blob-opslag downloaden:

* Op de **gegevens** menu, klikt u op **van tekst**.
* Blader naar het logboekbestand dat u wilt weergeven en klik op **importeren**.
* In stap 1 van de **Wizard Tekst importeren**, selecteer **gescheiden**.

In stap 1 van de **Wizard Tekst importeren**, selecteer **puntkomma** als het enige scheidingsteken en kiest u dubbele aanhalingstekens als de **tekstscheidingsteken**. Klik vervolgens op **voltooien** en geef aan waar de gegevens in uw werkmap te plaatsen.

### <a name="appendix-5"></a>Bijlage 5: Bewaking met Application Insights voor Visual Studio teamservices
U kunt ook de functie Application Insights voor Visual Studio Team Services gebruiken als onderdeel van de beschikbaarheidsbewaking van prestaties en. Dit hulpprogramma kunt doen:

* Zorg ervoor dat uw web-service beschikbaar is en reageert. Of uw app is een website of een apparaat-app die gebruikmaakt van een webservice, kunt u uw URL om de paar minuten testen vanaf locaties wereldwijd en laat u weten of er een probleem is.
* Analyseer snel eventuele prestatieproblemen of uitzonderingen in uw webservice. Ontdek als CPU of andere bronnen worden uitgerekt, krijgen die stack-traces van uitzonderingen en eenvoudig doorzoeken logboektraceringen. Als de prestaties van de app onder acceptabele grenzen, kunnen we u een e-mail sturen. U kunt zowel .NET en Java-web-services bewaken.

U vindt meer informatie op [wat is Application Insights](../../application-insights/app-insights-overview.md).

<!--Anchors-->
[Inleiding]: #introduction
[Rangschikking van deze handleiding]: #how-this-guide-is-organized

[bewaking van uw opslagservice]: #monitoring-your-storage-service
[Bewaking servicestatus]: #monitoring-service-health
[Bewaking van capaciteit]: #monitoring-capacity
[Controleprogramma beschikbaarheid]: #monitoring-availability
[Prestaties bewaken]: #monitoring-performance

[diagnose van opslagproblemen]: #diagnosing-storage-issues
[Health service-problemen]: #service-health-issues
[Prestatieproblemen]: #performance-issues
[Fouten opsporen]: #diagnosing-errors
[Emulator opslagproblemen]: #storage-emulator-issues
[Logboekregistratieprogramma's voor opslag]: #storage-logging-tools
[Met behulp van hulpprogramma's voor logboekregistratie]: #using-network-logging-tools

[End-to-end tracering]: #end-to-end-tracing
[Logboekgegevens correleren]: #correlating-log-data
[Aanvraag-ID van client]: #client-request-id
[Server aanvraag-ID]: #server-request-id
[Tijdstempels]: #timestamps

[richtlijnen voor probleemoplossing]: #troubleshooting-guidance
[metrische gegevens tonen AverageE2ELatency hoge en lage AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Prestatiegegevens geven lage AverageE2ELatency en lage AverageServerLatency aan, maar de client ondervindt hoge latentie]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Prestatiegegevens geven hoge AverageServerLatency aan]: #metrics-show-high-AverageServerLatency
[U ervaart onverwachte vertragingen bij de levering van berichten in een wachtrij]: #you-are-experiencing-unexpected-delays-in-message-delivery

[metrische gegevens tonen een toename in PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Tijdelijke toename van PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Permanente toename PercentThrottlingError fout]: #permanent-increase-in-PercentThrottlingError
[metrische gegevens tonen een toename in PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Prestatiegegevens geven een toename in PercentNetworkError aan]: #metrics-show-an-increase-in-PercentNetworkError

[De client ontvangt berichten HTTP 403 (verboden)]: #the-client-is-receiving-403-messages
[De client ontvangt berichten HTTP 404 (niet gevonden)]: #the-client-is-receiving-404-messages
[De client of een ander proces eerder verwijderd van het object]: #client-previously-deleted-the-object
[Een probleem met de autorisatie Shared Access Signature (SAS)]: #SAS-authorization-issue
[Client-side JavaScript-code is niet gemachtigd voor toegang tot het object]: #JavaScript-code-does-not-have-permission
[Netwerkfout]: #network-failure
[De client ontvangt berichten van de HTTP-409 (Conflict)]: #the-client-is-receiving-409-messages

[metrische gegevens tonen lage PercentSuccess of analytics logboekvermeldingen bewerkingen hebben met de status van ClientOtherErrors]: #metrics-show-low-percent-success
[Capaciteit metrische gegevens tonen een onverwachte toename in opslaggebruik capaciteit]: #capacity-metrics-show-an-unexpected-increase
[Er is sprake van virtuele Machines met een groot aantal gekoppelde VHD's onverwacht opnieuw wordt opgestart]: #you-are-experiencing-unexpected-reboots
[Het probleem zich voordoet de opslagemulator voor ontwikkeling of tests gebruiken]: #your-issue-arises-from-using-the-storage-emulator
[De functie 'X' werkt niet in de opslagemulator]: #feature-X-is-not-working
[Fout 'de waarde van een van de HTTP-headers heeft niet de juiste indeling' wanneer u de opslagemulator]: #error-HTTP-header-not-correct-format
[De opslagemulator moet beheerdersbevoegdheden]: #storage-emulator-requires-administrative-privileges
[U ondervindt problemen met het installeren van de Azure SDK voor .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[U hebt een ander probleem met storage-service]: #you-have-a-different-issue-with-a-storage-service

[bijlagen]: #appendices
[bijlage 1: HTTP en HTTPS-verkeer vastleggen met Fiddler]: #appendix-1
[bijlage 2: Wireshark gebruiken om vast te leggen netwerkverkeer]: #appendix-2
[bijlage 3: Microsoft Message Analyzer gebruiken om vast te leggen netwerkverkeer]: #appendix-3
[Bijlage 4: Met behulp van Excel metrische gegevens weergeven en gegevens aanmelden]: #appendix-4
[Bijlage 5: Bewaking met Application Insights voor Visual Studio teamservices]: #appendix-5

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
