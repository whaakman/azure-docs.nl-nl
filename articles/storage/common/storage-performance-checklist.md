---
title: Azure Storage-controlelijst voor prestaties en schaalbaarheid | Microsoft Docs
description: Een controlelijst met bewezen procedures voor gebruik met Azure Storage biedt praktische toepassingen te ontwikkelen.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 12/08/2016
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: d39c2414aa8299282b3896a9ceb57897fdb25ff1
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445998"
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage
## <a name="overview"></a>Overzicht
Sinds de release van de Microsoft Azure Storage-services, Microsoft heeft ontwikkeld, een aantal bewezen procedures voor het gebruik van deze services op een manier die goed presterende en in dit artikel dient de belangrijkste hiervan consolideren in een lijst controlelijst-stijl. De bedoeling van dit artikel is om te controleren of dat ze bewezen procedures gebruiken met Azure Storage voor ontwikkelaars van toepassingen en voor het identificeren van andere bewezen procedures die zij Overweeg gebruik te maken. In dit artikel wordt niet getracht te dekken van elke mogelijke optimalisatie van prestaties en schaalbaarheid, wordt die in hun invloed kleine of niet algemeen van toepassing zijn uitgesloten. Voor zover dit gedrag van de toepassing kan worden voorspeld tijdens het ontwerpen, is het handig om te blijven in gedachten vroeg om te voorkomen dat mogelijk te maken die wordt uitgevoerd in prestatieproblemen.  

Elke ontwikkelaar met behulp van Azure Storage moet even de tijd in dit artikel lezen en controleren van zijn of haar toepassing volgt elk van de bewezen procedures die hieronder worden vermeld.  

## <a name="checklist"></a>Controlelijst
In dit artikel worden de bewezen procedures in de volgende groepen ingedeeld. Bewezen procedures die van toepassing op:  

* Alle services van Azure Storage (blobs, tabellen, wachtrijen en bestanden)
* Blobs
* Tabellen
* Wachtrijen  

| Klaar | Onderwerp | Categorie | Vraag |
| --- | --- | --- | --- |
| &nbsp; | Alle services |Schaalbaarheidsdoelen |[Is uw toepassing zodanig ontworpen dat de schaalbaarheidsdoelen nadert?](#subheading1) |
| &nbsp; | Alle services |Schaalbaarheidsdoelen |[Is uw naamconventie ontworpen om in te schakelen beter taakverdeling?](#subheading47) |
| &nbsp; | Alle services |Netwerken |[Beschikt over clientapparaten kant voldoende hoge bandbreedte en lage latentie om de prestaties die nodig zijn?](#subheading2) |
| &nbsp; | Alle services |Netwerken |[Hebt clientapparaten naast een koppeling genoeg hoogwaardige?](#subheading3) |
| &nbsp; | Alle services |Netwerken |[Is de clienttoepassing zich 'in de buurt"het storage-account?](#subheading4) |
| &nbsp; | Alle services |Inhoudsdistributie |[Gebruikt u een CDN voor distributie van inhoud?](#subheading5) |
| &nbsp; | Alle services |Directe clienttoegang |[Gebruikt u SAS en CORS voor directe toegang tot opslag in plaats van proxy toestaan?](#subheading6) |
| &nbsp; | Alle services |Caching |[Uw gegevens opslaan in cache toepassing die meerdere malen wordt gebruikt en wijzigingen zelden is?](#subheading7) |
| &nbsp; | Alle services |Caching |[Is uw toepassing batchverwerking updates (deze clientzijde in cache en vervolgens geüpload in grotere sets)?](#subheading8) |
| &nbsp; | Alle services |Configuratie van .NET |[Hebt u de client voor het gebruik van een voldoende aantal gelijktijdige verbindingen geconfigureerd?](#subheading9) |
| &nbsp; | Alle services |Configuratie van .NET |[Hebt u .NET voor het gebruik van een voldoende aantal threads geconfigureerd?](#subheading10) |
| &nbsp; | Alle services |Configuratie van .NET |[Gebruikt u de .NET 4.5 of hoger, waarop garbagecollection is verbeterd?](#subheading11) |
| &nbsp; | Alle services |Parallelle uitvoering |[Hebt u gegarandeerd dat parallelle uitvoering op de juiste wijze wordt begrensd zodat u de clientmogelijkheden van uw of de schaalbaarheidsdoelen niet overbelast?](#subheading12) |
| &nbsp; | Alle services |Hulpprogramma's |[Vindt u de nieuwste versie van Microsoft-clientbibliotheken en hulpprogramma's?](#subheading13) |
| &nbsp; | Alle services |Nieuwe pogingen |[Weet u exponentieel uitstel met beleid voor het beperken van fouten en time-outs voor opnieuw proberen?](#subheading14) |
| &nbsp; | Alle services |Nieuwe pogingen |[Is uw toepassing vermijdt pogingen voor niet-herstelbare fouten?](#subheading15) |
| &nbsp; | Blobs |Schaalbaarheidsdoelen |[Hebt u een groot aantal clients tegelijkertijd toegang krijgen tot een enkel object?](#subheading46) |
| &nbsp; | Blobs |Schaalbaarheidsdoelen |[Is uw toepassing binnen het doel van de schaalbaarheid bandbreedte of bewerkingen voor één blob blijft?](#subheading16) |
| &nbsp; | Blobs |Blobs kopiëren |[Weet u blobs kopiëren op een efficiënte manier?](#subheading17) |
| &nbsp; | Blobs |Blobs kopiëren |[Gebruikt u AzCopy voor bulksgewijs kopieën van blobs?](#subheading18) |
| &nbsp; | Blobs |Blobs kopiëren |[Gebruikt u Azure Import/Export om over te dragen van grote hoeveelheden gegevens?](#subheading19) |
| &nbsp; | Blobs |Met behulp van metagegevens |[Bewaart u veelgebruikte metagegevens over blobs in de metagegevens?](#subheading20) |
| &nbsp; | Blobs |Snel uploaden |[Bij het snel uploaden van een blob, uploadt u blokken parallel?](#subheading21) |
| &nbsp; | Blobs |Snel uploaden |[Bij het snel veel blobs uploaden, uploadt u blobs parallel?](#subheading22) |
| &nbsp; | Blobs |Juiste Blobtype |[Gebruikt u pagina-blobs of blok-blobs wanneer dat nodig?](#subheading23) |
| &nbsp; | Tabellen |Schaalbaarheidsdoelen |[Bent u de schaalbaarheidsdoelen voor entiteiten per seconde bereikt?](#subheading24) |
| &nbsp; | Tabellen |Configuratie |[Gebruikt u JSON voor de tabel aanvragen?](#subheading25) |
| &nbsp; | Tabellen |Configuratie |[Hebt u uitgeschakeld Nagle verbeteren de prestaties van kleine aanvragen?](#subheading26) |
| &nbsp; | Tabellen |Tabellen en partities |[Uw gegevens goed gepartitioneerd?](#subheading27) |
| &nbsp; | Tabellen |Hot-partities |[Bent u alleen toevoegen en voeg alleen-lezen patronen te voorkomen?](#subheading28) |
| &nbsp; | Tabellen |Hot-partities |[Worden uw ingevoegd/updates verdeeld over veel partities?](#subheading29) |
| &nbsp; | Tabellen |Querybereik |[Hebt u een ontworpen voor uw schema om toe te staan voor point-query's moet worden gebruikt in de meeste gevallen en tabel query's moeten spaarzaam worden gebruikt?](#subheading30) |
| &nbsp; | Tabellen |Query dichtheid |[Uw doorgaans alleen controle van query's uitvoeren en retourneren van rijen die uw toepassing wordt gebruikt?](#subheading31) |
| &nbsp; | Tabellen |Geretourneerde gegevens beperken |[Gebruikt u filteren om te voorkomen dat er geretourneerd entiteiten die niet nodig zijn?](#subheading32) |
| &nbsp; | Tabellen |Geretourneerde gegevens beperken |[Gebruikt u projectie om te voorkomen dat er geretourneerd eigenschappen die niet nodig zijn?](#subheading33) |
| &nbsp; | Tabellen |Denormalisatie |[Hebt u uw gegevens gedenormaliseerd zodat u niet efficiënt query's of meerdere leesaanvragen voorkomen bij het ophalen van gegevens?](#subheading34) |
| &nbsp; | Tabellen |Invoegen, bijwerken en verwijderen |[Weet u batchverwerking aanvragen die moeten worden transactionele of kan worden uitgevoerd op hetzelfde moment om te beperken retouren?](#subheading35) |
| &nbsp; | Tabellen |Invoegen, bijwerken en verwijderen |[Bent u vermijden bij het ophalen van een entiteit die u zojuist hebt om te bepalen of aan te roepen invoegen of bijwerken?](#subheading36) |
| &nbsp; | Tabellen |Invoegen, bijwerken en verwijderen |[Hebt u de reeks gegevens die vaak worden opgehaald samen in één enkele entiteit opslaan als eigenschappen in plaats van meerdere entiteiten beschouwd?](#subheading37) |
| &nbsp; | Tabellen |Invoegen, bijwerken en verwijderen |[Voor entiteiten die altijd samen opgehaald en kunnen worden geschreven in batches (bijvoorbeeld: time series-gegevens), hebt u beschouwd als blobs in plaats van tabellen gebruiken?](#subheading38) |
| &nbsp; | Wachtrijen |Schaalbaarheidsdoelen |[Bent u de schaalbaarheidsdoelen voor berichten per seconde bereikt?](#subheading39) |
| &nbsp; | Wachtrijen |Configuratie |[Hebt u uitgeschakeld Nagle verbeteren de prestaties van kleine aanvragen?](#subheading40) |
| &nbsp; | Wachtrijen |Berichtgrootte |[Zijn uw compact berichten om de prestaties van de wachtrij te verbeteren?](#subheading41) |
| &nbsp; | Wachtrijen |Bulksgewijs ophalen |[Bent u bij het ophalen van meerdere berichten in één bewerking 'Ophalen'?](#subheading42) |
| &nbsp; | Wachtrijen |Pollingfrequentie opgeven |[Bent u polling vaak genoeg om te beperken van de waargenomen latentie van uw toepassing?](#subheading43) |
| &nbsp; | Wachtrijen |Bericht bijwerken |[Gebruikt u UpdateMessage voor het opslaan van voortgang in het verwerken van berichten, te voorkomen dat het hele bericht te verwerken als er een fout optreedt?](#subheading44) |
| &nbsp; | Wachtrijen |Architectuur |[Gebruikt u wachtrijen voor de gehele toepassing schaalbaarder maken door op te slaan langlopende workloads uit het kritieke pad en vervolgens onafhankelijk van elkaar schalen?](#subheading45) |

## <a name="allservices"></a>Alle Services
Deze sectie vindt u bewezen procedures die voor het gebruik van een van de Azure-opslagservices (blobs, tabellen, wachtrijen of bestanden gelden).  

### <a name="subheading1"></a>Schaalbaarheidsdoelen
Azure Storage zelf heeft een limiet van 250 storage-accounts per regio per abonnement. Als u deze limiet is bereikt, zich kunt u voor het maken van meer storage accounts in deze combinatie van abonnement/de regio.

Elk van de Azure Storage-services heeft schaalbaarheidsdoelen voor capaciteit (GB), transactietarief, en bandbreedte. Als uw toepassing nadert of hoger is dan een van de schaalbaarheidsdoelen, kan deze verbeterde transactie latenties of beperking optreden. Wanneer een Storage-service uw toepassing vertraagt, begint de service om terug te keren '503 Server bezet' of '500 time-out van de bewerking'-foutcodes voor sommige opslagtransacties. Deze sectie wordt de algemene benadering voor het omgaan met schaalbaarheidsdoelen- en prestatiedoelen voor bandbreedte in het bijzonder besproken. Latere secties die betrekking hebben op afzonderlijke opslagservices bespreken schaalbaarheidsdoelen in de context van die service:  

* [BLOB-bandbreedte en aanvragen per seconde](#subheading16)
* [Tabelitems per seconde](#subheading24)
* [Berichten per seconde](#subheading39)  

#### <a name="sub1bandwidth"></a>Schaalbaarheidsdoel bandbreedte voor alle Services
Op het moment van schrijven zijn de bandbreedte-doelen in de Verenigde Staten voor een account voor geografisch redundante opslag (GRS) 10 Gigabit per seconde (Gbps) voor inkomend (gegevens verzonden naar het opslagaccount) en 20 Gbps voor uitgaande gegevens (gegevens verzonden van het opslagaccount). Voor een account met lokaal redundante opslag (LRS), de limieten zijn hogere: 20 Gbps voor inkomend verkeer en 30 Gbps voor uitgaand verkeer.  Internationale bandbreedtelimieten mogelijk lager en kunt u vinden op onze [schaalbaarheid doelen pagina](https://msdn.microsoft.com/library/azure/dn249410.aspx).  Zie de koppelingen in nuttige informatiebronnen hieronder voor meer informatie over de opties voor opslagredundantie.  

#### <a name="what-to-do-when-approaching-a-scalability-target"></a>Wat te doen wanneer een schaalbaarheidsdoel nadert
Als de limiet van storage-accounts die u in de combinatie van een bepaald abonnement/de regio kunt hebben bijna is bereikt, uw toepassing en het gebruik van storage-accounts evalueren en te bepalen als een van deze voorwaarden van toepassing.

* Storage-accounts als niet-beheerde schijven gebruiken en deze schijven toe te voegen aan uw virtuele machines. In dit scenario wordt u aangeraden [beheerde schijven](../../virtual-machines/windows/managed-disks-overview.md), zoals deze schijf opslagschaalbaarheid voor u zonder dat u hoeft verwerken te maken en beheren van afzonderlijke storage-accounts.
* Met behulp van een storage-account op basis van per klant, ten behoeve van gegevensisolatie. In dit scenario raden wij het gebruik van storage-containers voor elke klant in plaats van een hele storage-account. Azure Storage nu kunt u opgeven van op rollen gebaseerd toegangsbeheer op een per [container basis](storage-auth-aad-rbac-portal.md).
* Gebruik meerdere opslagaccounts sharden voor grotere schaalbaarheid van Inkomend/uitgaand verkeer/iops/capaciteit. In dit scenario, indien mogelijk, raden wij u profiteren van de [verhoogd limieten](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) van standard storage-accounts te verminderen van het aantal opslagaccounts die zijn vereist voor uw workload.

Als uw toepassing de prestatiedoelen voor een enkel opslagaccount nadert, Overweeg de overstap naar een van de volgende methoden:  

* Heroverweeg de werkbelasting die ervoor zorgt dat uw toepassing om te benaderen of groter zijn dan het schaalbaarheidsdoel. Kunt u deze anders te gebruiken voor minder bandbreedte of capaciteit of minder transacties ontwerpen?
* Als een toepassing een van de schaalbaarheidsdoelen overschrijdt moet, moet u meerdere opslagaccounts en partitie uw toepassingsgegevens voor deze meerdere opslagaccounts. Als u dit patroon gebruiken, moet u ervoor dat u het ontwerp van uw toepassing, zodat u meer storage-accounts in de toekomst voor de taakverdeling kunt toevoegen. Elk Azure-abonnement kan maximaal 100 opslagaccounts hebben op het moment van schrijven.  Storage-accounts gelden ook geen kosten dan uw gebruik in termen van gegevens die zijn opgeslagen, transacties die zijn gemaakt of gegevens die worden overgedragen.
* Als uw toepassing de bandbreedte-doelen raakt, kunt u overwegen het comprimeren van gegevens in de client en het verminderen van de bandbreedte die vereist zijn de gegevens te verzenden naar de storage-service.  Hoewel dit mogelijk bandbreedte besparen en verbetering van netwerkprestaties, kan het ook sommige negatieve gevolgen hebben.  De invloed hiervan op de prestaties vanwege de extra verwerkingsvereisten voor het comprimeren en decomprimeren van gegevens in de client, moet u evalueren. Bovendien kan opslag van gecomprimeerde gegevens het moeilijker voor het oplossen van problemen, omdat het moeilijker om weer te geven van opgeslagen gegevens met standaardprogramma's kan worden.
* Als uw toepassing bij de schaalbaarheidsdoelen aankomt, zorg ervoor dat u exponentieel uitstel voor nieuwe pogingen gebruikt (Zie [nieuwe pogingen](#subheading14)).  Is het beter om te controleren of u nooit de schaalbaarheidsdoelen benadering (met behulp van een van de bovenstaande methoden), maar dit zorgt ervoor dat uw toepassing wordt niet alleen blijft proberen snel, waardoor de beperking slechter heeft.  

#### <a name="useful-resources"></a>Nuttige informatie
De volgende koppelingen bieden aanvullende details op schaalbaarheidsdoelen:

* Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor informatie over schaalbaarheidsdoelen.
* Zie [Azure Storage-replicatie](storage-redundancy.md) en de blogpublicatie [redundantieopties voor Azure-opslag en geografisch redundante opslag met leestoegang](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) voor informatie over opties voor opslagredundantie.
* Voor actuele informatie over de prijzen voor Azure-services, Zie [Azure-prijzen](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>Naamgevingsregels voor partities
Azure Storage maakt gebruik van een op bereiken gebaseerd partitieschema te schalen en taakverdeling het systeem. De partitiesleutel wordt gebruikt om te partitioneren van gegevens in de bereiken en deze bereiken zijn met load balancing in het systeem. Dit betekent dat naamgevingsregels zoals lexicale bestellen (bijvoorbeeld msftpayroll, msftperformance, msftemployees, enzovoort) of met behulp van tijdstempels (log20160101, log20160102, log20160102, enzovoort) wordt leent voor de partities mogelijk wordt geplaatst op de dezelfde partitie server totdat een bewerking load balancing ze uit in kleinere bereiken splitst. Alle blobs in een container kunnen bijvoorbeeld worden geleverd door één server, totdat de belasting op deze blobs is vereist meer herverdeling van de partitie-bereiken. Op dezelfde manier een groep van licht geladen accounts met hun namen lexicale volgorde kan worden geleverd door één server totdat de belasting van een of alle van deze accounts moet worden verdeeld over meerdere partities servers. Elke bewerking taakverdeling mogelijk van invloed op de latentie van storage aanroepen tijdens de bewerking. De mogelijkheid van het systeem voor het afhandelen van een plotselinge verhoging van het verkeer naar een partitie wordt beperkt door de schaalbaarheid van een server met één partitie totdat de bewerking load balancing gang in- en rebalances de partitiesleutelbereik.  

U kunt volgen enkele aanbevolen procedures voor het verminderen van de frequentie van dergelijke bewerkingen plaatsvindt.  

* Bekijk de naamconventie die u voor accounts, containers, blobs, tabellen en wachtrijen, nauw samen gebruiken. Houd rekening met het voorvoegsel van namen met een 3-cijferige hash met behulp van een hash-functie die het beste bij uw behoeften.  
* Als u uw gegevens met behulp van tijdstempels of numerieke id's ordenen, hebt u ervoor dat u niet een verkeerspatronen van waaraan alleen (of Voeg alleen-lezen). Deze patronen zijn niet geschikt voor een bereik-gebaseerd systeem te partitioneren en kan leiden tot al het verkeer naar één partitie te gaan en beperken van het systeem daadwerkelijk de taakverdeling. Bijvoorbeeld, als u de dagelijkse bewerkingen die gebruikmaken van een blob-object met een tijdstempel zoals JJJJMMDD hebt, wordt klikt u vervolgens al het verkeer voor die dag voor deze bewerking omgeleid naar een enkel object, dat wordt bediend door een server met één partitie. Bekijk of de blob-limieten per en per partitie limieten aan uw behoeften voldoet en kunt u overwegen om deze bewerking in meerdere blobs indien nodig. Op dezelfde manier als u de time series-gegevens in de tabellen opslaat, al het verkeer kan worden omgeleid naar het laatste deel van de belangrijkste naamruimte. Als u tijdstempels of numerieke id's moet gebruiken, het voorvoegsel van de ID met de hash van een 3 cijfers of in het geval van tijdstempels de seconden deel uit van de tijd, zoals ssyyyymmdd voorvoegsel. Als de aanbieding en bewerkingen uitvoeren van query's worden regelmatig uitgevoerd, kiest u een hash-functie die het aantal query's worden beperkt. In andere gevallen kan een willekeurige voorvoegsel voldoende zijn.  
* Voor meer informatie over het partitieschema gebruikt in Azure Storage, leest u het SOSP-document [hier](https://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Netwerken
Terwijl de API-materiaal aanroepen, hebben het fysieke netwerkbeperkingen van de toepassing vaak een aanzienlijke invloed op de prestaties. De volgende beschrijven een aantal beperkingen die gebruikers kunnen ondervinden.  

#### <a name="client-network-capability"></a>Client-netwerkmogelijkheden
##### <a name="subheading2"></a>Doorvoer
Voor bandbreedte is het probleem vaak de mogelijkheden van de client. Bijvoorbeeld, terwijl een enkel opslagaccount 10 Gbps of meer van inkomend verkeer kan verwerken (Zie [bandbreedte schaalbaarheidsdoelen](#sub1bandwidth)), de snelheid van het netwerk in een 'Kleine' Azure-Werkrol-instantie is alleen geschikt voor ongeveer 100 Mbps. Grotere Azure-exemplaren hebben NIC's met grotere capaciteit, moet u overwegen een groter rekenproces of meer virtuele machines als u hogere limieten voor netwerk vanaf één computer nodig hebt. Als u een Storage-service vanuit een on-premises-toepassing opent, wordt de dezelfde regel van toepassing is: meer informatie over de netwerkmogelijkheden van het clientapparaat en de netwerkverbinding met de Azure-opslaglocatie en een te verbeteren, indien nodig of het ontwerpen van uw toepassing werkt in hun mogelijkheden.  

##### <a name="subheading3"></a>Kwaliteit van de verbinding
Net als bij elke netwerkgebruik er rekening mee dat leidt tot fouten en pakketverlies netwerkomstandigheden effectieve doorvoer vertragen.  Met behulp van WireShark of NetMon kan helpen bij het vaststellen van dit probleem.  

##### <a name="useful-resources"></a>Nuttige informatie
Zie voor meer informatie over de grootten van virtuele machines en toegewezen bandbreedte [Windows VM-grootten](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [Linux VM-grootten](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>locatie
In een gedistribueerde omgeving biedt plaatst de client buurt bevindt van de server in de beste prestaties. Voor toegang tot Azure Storage met de laagste latentie, is de beste locatie voor de client binnen dezelfde Azure-regio. Als u een Azure-website die gebruikmaakt van Azure Storage hebt, moet u bijvoorbeeld beide binnen één regio (bijvoorbeeld VS West of Zuidoost-Azië). Dit vermindert de latentie en de kosten, op het moment van schrijven, bandbreedtegebruik binnen één regio is gratis.  

Als uw toepassingen zijn niet worden gehost in Azure (zoals apps voor mobiele apparaten of lokale enterprise-Services), nog een keer client plaatsen van het storage-account in een regio buurt bevindt van de apparaten die toegang heeft tot deze wordt in het algemeen Verminder de latentie. Als uw clients worden globaal gedistribueerd (voor bijvoorbeeld enkele in Noord-Amerika, en sommige in Europa), dan moet u overwegen meerdere opslagaccounts: een zich in een regio Noord-Amerikaanse en één in een Europese regio. Dit helpt te verminderen latentie voor gebruikers in beide regio's. Deze aanpak is eenvoudiger te implementeren als de gegevens door de toepassing opgeslagen is specifiek voor afzonderlijke gebruikers, en is niet vereist voor het repliceren van gegevens tussen de storage-accounts.  Brede distributie van inhoud, een CDN wordt aanbevolen – Zie de volgende sectie voor meer informatie.  

### <a name="subheading5"></a>Distributie van inhoud
Soms moet een toepassing dezelfde inhoud aan meerdere gebruikers (bijvoorbeeld een product demovideo gebruikt in de startpagina van een website), zich in hetzelfde bestand of meerdere regio's in te dienen. In dit scenario, moet u een Content Delivery Network (CDN), zoals Azure CDN gebruiken en het CDN gebruik Azure-opslag als de oorsprong van de gegevens. In tegenstelling tot een Azure Storage-account die zich in een enkele regio en die niet kan leveren van inhoud met een lage latentie tot andere regio's, gebruikt Azure CDN servers in meerdere datacenters over de hele wereld. Bovendien kan een CDN meestal veel hoger uitgaande limieten dan één opslagaccount ondersteunen.  

Zie voor meer informatie over Azure CDN [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>Met behulp van SAS en CORS
Als u toestaan, zoals JavaScript-code in de webbrowser van een gebruiker of een mobiele telefoon-app voor toegang tot gegevens in Azure Storage wilt, een aanpak is het gebruik van een toepassing in de Webrol als een proxy: het apparaat van de gebruiker wordt geverifieerd met de Webrol , die op zijn beurt gemachtigd voor toegang tot opslagresources. Op deze manier kunt u voorkomen dat uw storage-accountsleutels op onveilige apparaten. Echter, plaatst deze een aanzienlijke overhead op de Webrol omdat alle gegevens die overgedragen tussen het apparaat van de gebruiker en de storage-service aan de Webrol doorgeven moet. U kunt vermijden met behulp van een Webrol als proxy voor de storage-service met behulp van de Shared Access Signatures (SAS), soms in combinatie met kopteksten Cross-Origin Resource Sharing (CORS). Met behulp van SAS, kunt u het apparaat van uw gebruiker om aan te vragen rechtstreeks naar een storage-service met behulp van een token met beperkte toegang toestaan. Bijvoorbeeld, als een gebruiker wil een foto uploaden naar uw toepassing, uw Webrol kunt genereren en verzenden naar het apparaat van de gebruiker een SAS-token dat een machtiging verleend voor het schrijven naar een specifieke blob of container voor de komende 30 minuten (waarna de SAS-token verloopt).

Een browser wordt normaal gesproken niet JavaScript toestaan in een pagina die wordt gehost door een website op één domein bepaalde bewerkingen, zoals een 'plaats' naar een ander domein uit te voeren. Als u een Webrol op "contosomarketing.cloudapp.net" hosten en gebruiken aan de clientzijde JavaScript wilt naar een blob geüpload naar uw opslagaccount op 'contosoproducts.blob.core.windows.net', wordt de browsers 'beleid voor dezelfde oorsprong' deze bewerking verbieden. CORS is een browser-functie waarmee het doeldomein (in dit geval de storage-account) om te communiceren met de browser dat wordt vertrouwd aanvragen die afkomstig zijn van het brondomein (in dit geval de Webrol).  

Beide van deze technologieën kunt u onnodige belasting (en knelpunten) voorkomen in uw webtoepassing.  

#### <a name="useful-resources"></a>Nuttige informatie
Zie voor meer informatie over SAS [handtekeningen voor gedeelde toegang, deel 1: Inzicht in het SAS-Model](../storage-dotnet-shared-access-signature-part-1.md).  

Zie voor meer informatie over CORS [Cross-Origin Resource Sharing (CORS) ondersteuning voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Caching
#### <a name="subheading7"></a>Getting Data
In het algemeen is het beter dan twee keer aan voor het ophalen van gegevens uit een service is het één keer. Bekijk het voorbeeld van een MVC-webtoepassing die wordt uitgevoerd in een Webrol die al een blob 50 MB van de storage-service om te fungeren als inhoud aan een gebruiker opgehaald. De toepassing kan vervolgens die dezelfde blob ophalen telkens wanneer een gebruiker om vraagt, of deze kan opslaan in cache deze lokaal naar schijf en de versie van de cache voor latere gebruikersaanvragen hergebruiken. Bovendien telkens wanneer een gebruiker vraagt de gegevens, de toepassing kan probleem ophalen met een voorwaardelijke header voor tijd van wijziging, die te voorkomen de hele blob dat zou als deze nog niet is gewijzigd. U kunt deze hetzelfde patroon toepassen op het werken met tabelentiteiten.  

In sommige gevallen kunt u bepalen dat uw toepassing wordt ervan uitgegaan dat de blob is geldig gedurende een korte periode na het ophalen van het, en dat gedurende deze periode de toepassing hoeft niet te controleren als de blob is gewijzigd.

Configuratie, opzoeken en andere gegevens die altijd worden gebruikt door de toepassing zijn goede kandidaten voor opslaan in cache.  

Zie voor een voorbeeld van hoe u aan de eigenschappen van een blob voor het detecteren van de datum van laatste wijziging met behulp van .NET [instellen en ophalen van eigenschappen en metagegevens](../blobs/storage-properties-metadata.md). Zie voor meer informatie over voorwaardelijke downloads [voorwaardelijk vernieuwen van een lokale kopie van een Blob](https://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Uploaden van gegevens in Batches
In bepaalde situaties toepassing, kunt u statistische gegevens lokaal en vervolgens regelmatig uploaden in een batch in plaats van elke hoeveelheid gegevens direct uploaden. Bijvoorbeeld, een webtoepassing ervoor kiezen om een logbestand van activiteiten: de toepassing kan de details van elke activiteit uploaden als dit als een Tabelentiteit gebeurt (hiervoor veel opslagbewerkingen) of het kan de details van computeractiviteit opslaan naar een lokale logboekbestand, en vervolgens regelmatig alle details van computeractiviteit als een bestand met scheidingstekens naar een blob uploaden. Als elke logboekvermelding 1 KB groot is, kunt u duizenden in één 'Put Blob' transactie (u kunt een blob van 64 MB in grootte in één transactie uploaden) uploaden. Als de lokale computer voordat het uploaden vastloopt, wordt u mogelijk enkele logboekgegevens verbroken: ontwikkelaar van de toepassing moet ontwerpen voor de mogelijkheid van client-apparaat of uploadfouten.  Als de activiteitsgegevens moeten worden gedownload voor timespans (niet slechts één activiteit), worden de blobs voorkeur boven tabellen.

### <a name="net-configuration"></a>Configuratie van .NET
Als u .NET Framework gebruikt, vindt deze sectie u enkele snelle configuratie-instellingen die u gebruiken kunt om belangrijke prestatieverbeteringen.  Als u andere talen, controleert u of vergelijkbare concepten toepassing in uw gekozen taal.  

#### <a name="subheading9"></a>De standaardlimiet voor de verbinding verhogen
In .NET neemt de volgende code de standaard verbindingslimiet (dit is doorgaans 2 in de clientomgeving van een of 10 in een serveromgeving) tot 100. Normaal gesproken moet u de waarde ingesteld op ongeveer het aantal threads die worden gebruikt door uw toepassing.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

U moet de limiet voor verbindingen voor het openen van alle verbindingen instellen.  

Zie de die taal documentatie om te bepalen hoe de verbindingslimiet instellen voor andere programmeertalen.  

Zie voor meer informatie het blogbericht [Web Services: Gelijktijdige verbindingen](https://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>ThreadPool Min Threads te verhogen als synchrone code met asynchrone taken
Deze code wordt de thread pool min-threads te verhogen:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Zie voor meer informatie, [ThreadPool.SetMinThreads methode](https://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>Profiteer van .NET 4.5 garbagecollection
Gebruik .NET 4.5 of hoger voor de clienttoepassing om te profiteren van verbeterde prestaties in server garbagecollection.

Zie voor meer informatie het artikel [een overzicht van de prestatieverbeteringen in .NET 4.5](https://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Niet-gebonden parallelle uitvoering
Hoewel parallelle uitvoering ideaal voor prestaties zijn kan, Wees voorzichtig met het gebruik van niet-gebonden parallelle uitvoering (geen limiet voor het aantal threads en/of parallelle aanvragen) om te uploaden of downloaden van gegevens, met behulp van meerdere werknemers toegang tot meerdere partities (containers, wachtrijen, of Tabelpartities) in hetzelfde opslagaccount of voor toegang tot meerdere items in dezelfde partitie. Als de parallelle uitvoering niet-gebonden is, uw toepassing kan groter zijn dan het clientapparaat mogelijkheden of schaalbaarheid van het opslagaccount dat is gericht op wat resulteert in langere latentietijden en beperking.  

### <a name="subheading13"></a>Storage-clientbibliotheken en hulpprogramma 's
Gebruik altijd de meest recente Microsoft-clientbibliotheken en hulpprogramma's. Op het moment van schrijven, moet u er-clientbibliotheken beschikbaar voor .NET, Windows Phone, Windows-Runtime, Java en C++, evenals de preview-bibliotheken voor andere talen zijn. Bovendien heeft Microsoft PowerShell-cmdlets en Azure CLI-opdrachten voor het werken met Azure Storage uitgebracht. Microsoft actief deze hulpprogramma's waarvan prestaties zijn er rekening mee ontwikkelt, houdt u deze up-to-date zijn met de meest recente serviceversies en zorgt ervoor dat ze veel van de bewezen prestaties intern verwerkt.  

### <a name="retries"></a>Nieuwe pogingen
#### <a name="subheading14"></a>Beperking/ServerBusy
In sommige gevallen kan de storage-service kan uw toepassing beperken of gewoon mogelijk geen dient voor de aanvraag vanwege een tijdelijke toestand en een bericht '503 Server bezet' of '500 time-out' retourneren.  Dit kan gebeuren als uw toepassing een van de schaalbaarheidsdoelen nadert, of als het systeem is herverdeling de gepartitioneerde gegevens om toe te staan voor een hogere doorvoer.  De clienttoepassing moet meestal probeer het opnieuw die ervoor zorgt een fout opgetreden dat: er wordt later opnieuw geprobeerd dezelfde aanvraag kan worden uitgevoerd. Echter, als de storage-service is uw toepassing beperking, omdat deze schaalbaarheidsdoelen overschrijdt, of zelfs als de service kon geen voor de aanvraag om een andere reden, agressief nieuwe pogingen meestal ervoor het probleem nog erger. Daarom moet u een exponentieel uitstel (de client-bibliotheken standaard dit gedrag). Uw toepassing kan bijvoorbeeld opnieuw proberen na 2 seconden dan 4 seconden en vervolgens 10 seconden, dan 30 seconden en volledig van geven. Dit gedrag resulteert in uw toepassing aanzienlijk verminderen van de belasting van de service in plaats van exacerbating eventuele problemen.  

Fouten in de basisnetwerkverbinding kunnen opnieuw worden geprobeerd onmiddellijk, omdat ze niet het resultaat van de beperking en tijdelijk worden verwacht.  

#### <a name="subheading15"></a>Niet-herstelbare fouten
De clientbibliotheken zich bewust zijn van welke fouten probeer het opnieuw te kunnen zijn en welke niet. Echter, als u uw eigen code op basis van de storage REST-API, houd er rekening mee er zijn enkele fouten die u moet niet opnieuw proberen: bijvoorbeeld 400 (foute aanvraag) antwoord geeft aan dat de clienttoepassing een aanvraag kan niet worden verwerkt omdat deze niet verzonden een verwachte indeling. Opnieuw verzenden van deze aanvraag wordt verwijderd, hetzelfde antwoord telkens, dus er geen punt is in het opnieuw proberen. Als u uw eigen code op basis van de storage REST-API schrijft, worden op de hoogte van de betekenis van de foutcodes en de juiste manier voor het opnieuw proberen (of uitschakelen) voor elk van deze.  

#### <a name="useful-resources"></a>Nuttige informatie
Zie voor meer informatie over foutcodes voor opslag, [Status en foutcodes](https://msdn.microsoft.com/library/azure/dd179382.aspx) op de website van Microsoft Azure.  

## <a name="blobs"></a>Blobs
Naast de bewezen procedures voor het [alle Services](#allservices) eerder is beschreven, de volgende beproefde procedures name van toepassing op de blob-service.  

### <a name="blob-specific-scalability-targets"></a>Schaalbaarheidsdoelen van BLOB-specifieke
#### <a name="subheading46"></a>Meerdere clients tegelijkertijd toegang krijgen tot een enkel object
Als u een groot aantal clients tegelijkertijd toegang krijgen tot een enkel object hebt, moet u rekening houden met per schaalbaarheidsdoelen voor object en storage-account. Het exacte aantal clients die toegang hebben tot een enkel object wordt variëren, afhankelijk van factoren zoals het aantal clients tegelijkertijd aanvragen van het object de grootte van het object, netwerk-voorwaarden enzovoort.

Als het object kan worden gedistribueerd via een CDN, zoals afbeeldingen en video's worden aangeleverd vanuit een website en vervolgens moet u een CDN. Zie [hier](#subheading5).

In andere scenario's zoals wetenschappelijke simulaties waar de gegevens vertrouwelijk zijn hebt u twee opties. De eerste is de toegang van uw workload spreiden zodanig dat het object is geopend gedurende een periode van tegenover gelijktijdig toegankelijk zijn. U kunt het object ook tijdelijk naar meerdere opslagaccounts verhogen de totale IOP's per object of tussen opslagaccounts kopiëren. In het beperkte testen we gevonden dat ongeveer 25 virtuele machines tegelijkertijd een blob van 100 GB (elke virtuele machine is bronresourcepartities gelijktijdig te downloaden met behulp van 32 threads) gelijktijdig kunnen downloaden. Als u beschikt over 100 clients die toegang tot het object, eerst kopiëren naar een tweede storage-account en vervolgens de eerste 50 virtuele machines toegang tot de eerste blob en de tweede 50 VM's toegang tot de tweede blob. Resultaten zullen verschillen afhankelijk van het gedrag van uw toepassingen, zodat u deze tijdens het ontwerpen testen moet. 

#### <a name="subheading16"></a>Bandbreedte en -bewerkingen per Blob
U kunt lezen of schrijven naar één blob op tot een maximum van 60 MB per seconde (dit is ongeveer 480 Mbps, die groter is dan de mogelijkheden van veel client-side-netwerken (met inbegrip van de fysieke NIC op de client-apparaat). Bovendien ondersteunt één blob maximaal 500 aanvragen per seconde. Als u meerdere clients hebt die moeten dezelfde blob lezen hebt en u kunt deze limieten overschrijdt, moet u overwegen een CDN voor het distribueren van de blob.  

Zie voor meer informatie over doeldoorvoer van blobs, [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Kopiëren en verplaatsen van Blobs
#### <a name="subheading17"></a>Blob kopiëren
De REST API-versie 2012-02-12-opslag is de mogelijkheid handig om te kopiëren van blobs voor accounts geïntroduceerd: een clienttoepassing kunt instrueren de storage-service naar een blob kopiëren vanaf een andere bron (mogelijk in een ander opslagaccount) en vervolgens laat u de service uitvoeren de kopie asynchroon. Dit kan aanzienlijk verminderen de bandbreedte die nodig is voor de toepassing wanneer u gegevens uit andere opslagaccounts migreert omdat u niet wilt downloaden en de gegevens te uploaden.  

Een van die afwegingen, is echter dat bij het kopiëren tussen opslagaccounts, er geen garantie tijd is op waarop het exemplaar wordt voltooid. Als uw toepassing een blob-kopie snel onder uw beheer te voltooien moet, is het wellicht beter is om te kopiëren van de blob aan een virtuele machine downloaden en vervolgens geüpload naar de bestemming.  Volledige te voorspellen in dit geval, zorg ervoor dat het exemplaar wordt uitgevoerd door een virtuele machine die wordt uitgevoerd in dezelfde Azure-regio, anders netwerkomstandigheden invloed op de prestaties van uw exemplaar kan (en waarschijnlijk wordt).  Bovendien kunt u de voortgang van een asynchrone kopie programmatisch bewaken.  

Exemplaren binnen hetzelfde opslagaccount zelf wordt algemeen snel uitgevoerd.  

Zie voor meer informatie, [kopiëren van de Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>AzCopy gebruiken
Het Azure Storage-team heeft een opdrachtregelprogramma 'AzCopy' die is bedoeld om u te helpen bij bulksgewijs overbrengen van veel blobs naar, van, en voor opslagaccounts.  Dit hulpprogramma is geoptimaliseerd voor dit scenario en kan maar liefst maximale overdrachtssnelheid.  Het gebruik ervan wordt aangemoedigd voor bulksgewijs uploaden, downloaden en kopiëren scenario's. Zie voor meer informatie over deze en downloaden, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Azure Import/Export-Service
Voor grote hoeveelheden gegevens (meer dan 1 TB) biedt Azure Storage de Import/Export-service, waarmee voor het uploaden en downloaden van blob-opslag van verzending van harde schijven.  U kunt uw gegevens op een harde schijf plaatsen en naar Microsoft verzenden voor het uploaden of een lege harde schijf te verzenden naar Microsoft om gegevens te downloaden.  Zie [De Microsoft Azure Import/Export-service gebruiken om gegevens over te brengen naar Blob Storage](../storage-import-export-service.md) voor meer informatie.  Dit kan veel efficiënter dan het uploaden/downloaden van dit volume gegevens via het netwerk zijn.  

### <a name="subheading20"></a>Met behulp van metagegevens
De blob-service ondersteunt head-aanvragen die metagegevens over de blob bevatten kunnen. Bijvoorbeeld, als uw toepassing nodig de EXIF-gegevens buiten een foto, kan deze ophalen van de foto en pak het uit. Bandbreedte besparen en de prestaties verbeteren, uw toepassing kan opslaan van de EXIF-gegevens in de metagegevens van de blob wanneer de toepassing de foto geüpload: u kunt vervolgens halen de EXIF-gegevens in de metagegevens met behulp van alleen een HEAD-aanvraag, veel bandbreedte, opslaan en de van verwerkingstijd die nodig is om op te halen van de EXIF-gegevens die telkens wanneer de blob is gelezen. Dit zou zijn nuttig in scenario's waarin u alleen de metagegevens en niet de volledige inhoud van een blob hoeft.  Alleen 8 KB van de metagegevens van kan per blob (de service wordt een aanvraag voor het opslaan van meer dan die niet accepteren), worden opgeslagen, zodat als de gegevens niet in die grootte past, kunt u mogelijk niet gebruiken van deze benadering.  

Zie voor een voorbeeld van het ophalen van een blob-metagegevens met behulp van .NET, [instellen en ophalen van eigenschappen en metagegevens](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Snel uploaden
Als u wilt snel blobs uploaden, de eerste vraag te beantwoorden is: u uploadt een blob of veel zijn?  Gebruik de onderstaande instructies om te bepalen van de juiste methode te gebruiken, afhankelijk van uw scenario.  

#### <a name="subheading21"></a>Snel uploaden een grote blob
Als u wilt één grote blob snel uploaden, moet u de clienttoepassing de blokken of pagina's parallel (wordt gelet op de schaalbaarheidsdoelen voor afzonderlijke blobs en de storage-account als geheel) uploaden.  De officiële Microsoft geleverde RTM Storage-clientbibliotheken (.NET, Java) hebben de mogelijkheid om dit te doen.  Voor elk van de bibliotheken, gebruikt u de hieronder opgegeven object/eigenschap om in te stellen de mate van gelijktijdigheid:  

* .NET: ParallelOperationThreadCount instellen voor een BlobRequestOptions-object moet worden gebruikt.
* Java/Android: Use BlobRequestOptions.setConcurrentRequestCount()
* Node.js: Gebruik parallelOperationThreadCount op een van beide Aanvraagopties voor de of op de blob-service.
* C++: Gebruik de methode blob_request_options::set_parallelism_factor.

#### <a name="subheading22"></a>Veel blobs zijn uploaden snel
Om u te veel blobs zijn snel uploaden, blobs parallel te uploaden. Dit is sneller dan één blobs uploaden op een tijdstip via parallel blok uploads, omdat het uploaden van de verspreidt over meerdere partities van de storage-service. Één blob biedt alleen ondersteuning voor een doorvoer van 60 MB per seconde (ongeveer 480 Mbps). Op het moment van schrijven, wordt een VS gebaseerde LRS-account ondersteunt maximaal 20 Gbps inkomend verkeer dat nog veel meer is dan de doorvoer die wordt ondersteund door een afzonderlijke blob is.  [AzCopy](#subheading18) uploads parallel standaard uitvoert, en wordt aanbevolen voor dit scenario.  

### <a name="subheading23"></a>Het juiste type blob kiezen
Azure Storage ondersteunt twee typen blob: *pagina* blobs en *blok* blobs. Uw keuze van het blobtype heeft invloed op de prestaties en schaalbaarheid van uw oplossing voor een bepaalde gebruiksscenario. Blok-blobs zijn geschikt als u wilt uploaden van grote hoeveelheden gegevens efficiënt: bijvoorbeeld mogelijk een clienttoepassing foto's of video uploaden naar blob-opslag. Pagina-blobs geschikt als de toepassing nodig heeft om uit te voeren van willekeurige schrijfbewerkingen op de gegevens zijn: bijvoorbeeld Azure-VHD's worden opgeslagen als pagina-blobs.  

Zie voor meer informatie, [blok-Blobs, toevoeg-Blobs en pagina-Blobs](https://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tabellen
Naast de bewezen procedures voor het [alle Services](#allservices) eerder is beschreven, de volgende beproefde procedures name van toepassing op de table-service.  

### <a name="subheading24"></a>Tabel-specifieke Schaalbaarheidsdoelen
Tabellen hebben naast de bandbreedtebeperkingen van een hele storage-account, de volgende specifieke schaalbaarheidslimiet.  Het systeem wordt verdelen als uw verkeer toeneemt, maar als uw verkeer plotselinge pieken heeft, kunt u mogelijk niet onmiddellijk ophalen van dit volume aan doorvoer.  Als het patroon gekenmerkt door bursts, moet u verwachten dat om te beperken en/of time-outs tijdens de burst als de opslag-service automatisch load saldo van uw tabel zien.  Langzaam in het algemeen verbeteren heeft betere resultaten als deze beschikken over de systeemtijd op de juiste wijze verdelen.  

#### <a name="entities-per-second-account"></a>Entiteiten per seconde (Account)
De schaalbaarheidslimiet voor toegang tot tabellen is maximaal 20.000 entiteiten (1 KB elke) per seconde voor een account.  In het algemeen elke entiteit die wordt ingevoegd, bijgewerkt, verwijderd of gescand telt mee voor dit doel.  Een batch insert die 100 entiteiten bevat zou dus meegeteld als 100 entiteiten.  Een query die 1000 entiteiten scant en retourneert 5 wordt meegeteld als 1000 entiteiten.  

#### <a name="entities-per-second-partition"></a>Entiteiten per seconde (partitie)
Binnen een enkele partitie is het schaalbaarheidsdoel voor toegang tot tabellen 2000 entiteiten (1 KB elke) per seconde, met behulp van de dezelfde tellen zoals beschreven in de vorige sectie.  

### <a name="configuration"></a>Configuratie
Deze sectie vindt u enkele snelle configuratie-instellingen die u gebruiken kunt om belangrijke prestatieverbeteringen in de table-service maken:  

#### <a name="subheading25"></a>JSON gebruiken
Vanaf versie van de service storage 2013-08-15, ondersteunt de table-service het gebruik van JSON in plaats van de AtomPub op basis van een XML-indeling voor het overbrengen van gegevens in een tabel. Dit nettoladingen met minder dan 75% kan verlagen en kan de prestaties van uw toepassing aanzienlijk verbeteren.

Zie voor meer informatie het bericht [Microsoft Azure-tabellen: Inleiding tot JSON](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) en [indeling nettolading voor servicebewerkingen tabel](https://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle uitschakelen
Van Nagle algoritme wordt breed geïmplementeerd via TCP/IP-netwerken als ter verbetering van netwerkprestaties. Het is echter niet optimaal zijn in alle gevallen (zoals zeer interactieve omgevingen). Van Nagle-algoritme heeft een nadelige invloed op de prestaties van aanvragen voor de tabel en wachtrij-services voor Azure Storage, en moet u dit indien mogelijk uitschakelen.  

Zie voor meer informatie onze blogbericht [van Nagle algoritme is niet beschrijvende voor kleine aanvragen](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), waarin wordt uitgelegd waarom van Nagle algoritme slecht communiceert met de tabel en wachtrij aanvragen en laat zien hoe u deze in uw client uitschakelen de toepassing.  

### <a name="schema"></a>Schema
Is de grootste één factor die van invloed is op de prestaties van de table-service het vertegenwoordigen en uw gegevens op te vragen. Hoewel elke toepassing verschilt, is deze sectie geeft een overzicht enkele algemene bewezen procedures die betrekking hebben op:  

* Tabelontwerp
* Efficiënte query 's
* Efficiënte Gegevensupdates  

#### <a name="subheading27"></a>Tabellen en partities
Tabellen zijn onderverdeeld in partities. Elke entiteit die zijn opgeslagen in een partitie deelt dezelfde partitiesleutel en een unieke rij-sleutel bij het identificeren ervan in de betreffende partitie heeft. Partities voordelen bieden, maar ook limieten voor schaalbaarheid introduceren.  

* Voordelen: U kunt entiteiten in dezelfde partitie in een enkele, atomisch, batchtransactie met maximaal 100 afzonderlijke-bewerkingen (maximaal 4 MB totale grootte) bijwerken. Ervan uitgaande dat hetzelfde aantal entiteiten worden opgehaald, u kunt ook een query gegevens binnen een enkele partitie efficiënter dan gegevens meerdere partities (Hoewel lezen op voor verdere aanbevelingen over het opvragen van tabelgegevens).
* Schaalbaarheidslimiet voor: Toegang tot entiteiten die zijn opgeslagen in één partitie kan niet worden met load balancing omdat partities atomische batchtransacties ondersteunt. Daarom is het schaalbaarheidsdoel voor de partitie van een afzonderlijke tabel lager is dan voor de table-service als geheel.  

Vanwege deze kenmerken van tabellen en partities, moet u de volgende ontwerpprincipes nemen:  

* Gegevens die regelmatig bijgewerkt of query's uitgevoerd in dezelfde logische eenheid van het werk uw clienttoepassing moet zich bevinden in dezelfde partitie.  Dit kan zijn omdat uw toepassing wordt geaggregeerd schrijfbewerkingen, of omdat u wilt profiteren van atomic batchbewerkingen.  Ook, gegevens in één partitie kan efficiënt query's uitvoeren in één query dan gegevens over meerdere partities.
* Gegevens die via de clienttoepassing heeft niet invoegen/bijwerken of query's in dezelfde logische eenheid van het werk (één query of batch-update) moet zich bevinden in afzonderlijke partities.  Een belangrijke opmerking is dat er geen limiet voor het aantal partitiesleutels in één tabel, zodat met miljoenen partitiesleutels geen probleem is en is niet van invloed op prestaties.  Bijvoorbeeld, als uw toepassing een populaire website met gebruikersaanmelding is, is met behulp van de gebruikers-ID als de partitiesleutel een goede keuze.  

#### <a name="hot-partitions"></a>Hot-partities
Een hot partitie is een die een onevenredige percentage van het verkeer naar een rekening ontvangt en kan niet worden met gelijke taakverdeling omdat het een enkele partitie.  Hot-partities worden in het algemeen, twee manieren gemaakt:  

##### <a name="subheading28"></a>Alleen toevoegen en voeg alleen patronen
Het patroon 'Alleen toevoegen' is een waar alle (of bijna alle) van het verkeer naar een bepaalde PK toeneemt en afneemt volgens de huidige tijd.  Een voorbeeld is als uw toepassing de huidige datum als de partitiesleutel van een voor logboekgegevens gebruikt.  Dit resulteert in alle van de invoegingen gaan naar de laatste partitie in uw tabel, en het systeem kan saldo niet laden omdat alle van de schrijfbewerkingen aan het einde van de tabel gaat.  Als het volume van het verkeer naar deze partitie is groter dan het schaalbaarheidsdoel van de partitie-niveau van, en vervolgens het resultaat beperking.  Is het beter om ervoor te zorgen dat verkeer wordt verzonden naar meerdere partities, waarmee taakverdeling de aanvragen in de tabel.  

##### <a name="subheading29"></a>Gegevens van veel verkeer
Als uw partitieschema in een enkele partitie waaraan alleen gegevens die veel meer dan andere partities worden gebruikt resulteert, ziet u mogelijk ook als beperking dat partitie het schaalbaarheidsdoel voor een enkele partitie nadert.  Is het beter om ervoor te zorgen dat uw partitieschema resulteert in geen enkele partitie nadert de prestatiedoelen.  

#### <a name="querying"></a>Uitvoeren van query's
Deze sectie beschrijft bewezen procedures voor het uitvoeren van query's de table-service.  

##### <a name="subheading30"></a>Querybereik
Er zijn verschillende manieren om op te geven van het bereik van entiteiten aan query.  Hier volgt een beschrijving van het gebruik van elk.  

In het algemeen te voorkomen dat scans (query's groter is dan één entiteit), maar als u scannen moet, probeert de gegevens te ordenen zodat uw scans ophalen van de gegevens die u nodig hebt zonder scannen of het opvragen van aanzienlijke hoeveelheden entiteiten die u niet nodig hebt.  

###### <a name="point-queries"></a>Point-query 's
Een point-query wordt precies één entiteit opgehaald. Dit gebeurt door het opgeven van zowel de partitiesleutel en rijsleutel van de entiteit om op te halen. Deze query's efficiënt zijn en moet u ze gebruiken waar mogelijk.  

###### <a name="partition-queries"></a>Partitie-query 's
Een partitiequery voor de is een query waarmee een set gegevens die een algemene partitiesleutel deelt worden opgehaald. De query is meestal een bereik van de sleutelwaarden rij of een bereik van waarden voor de entiteitseigenschap van een naast een partitiesleutel. Dit zijn minder efficiënt dan point-query's en moeten spaarzaam worden gebruikt.  

###### <a name="table-queries"></a>Tabel query 's
Een query uitvoeren op tabel is een query waarmee een set standaardentiteiten die een algemene partitiesleutel niet deelt worden opgehaald. Deze query's zijn niet efficiënt en u indien mogelijk moet voorkomen.  

##### <a name="subheading31"></a>Query dichtheid
Een andere belangrijke factor in de efficiëntie van de query is het aantal entiteiten in vergelijking met het aantal gescand om de resulterende set entiteiten geretourneerd. Als uw toepassing een query uitvoeren op tabel met een filter voor een eigenschapswaarde voert dat slechts 1% van de gegevensshares, de query worden gescand 100 entiteiten voor elke één entiteit die wordt geretourneerd. De tabel schaalbaarheidsdoelen besproken eerder alle hebben betrekking op het aantal entiteiten die zijn gescand, en niet het aantal entiteiten geretourneerd: een dichtheid lage query kan eenvoudig ertoe leiden dat de table-service om te beperken van uw toepassing, omdat deze moet zo veel entiteiten om te scannen de entiteit die u zoekt ophalen.  Zie de sectie hieronder op [denormalisatie](#subheading34) voor meer informatie over hoe u dit probleem te voorkomen.  

##### <a name="limiting-the-amount-of-data-returned"></a>De hoeveelheid geretourneerde gegevens beperken
###### <a name="subheading32"></a>Filtering
Wanneer u weet dat een query entiteiten die u hoeft niet in de clienttoepassing wordt geretourneerd, kunt u overwegen een filter om de grootte van de resulterende set te beperken. Terwijl de entiteiten die niet zijn geretourneerd naar de client wordt nog steeds mee voor de limieten voor schaalbaarheid tellen, verbetert de toepassingsprestaties van uw vanwege de grootte van de nettolading verminderde netwerk en de verminderde aantal entiteiten waarmee de clienttoepassing moet worden verwerkt.  Houd er rekening mee hierboven op [Query dichtheid](#subheading31), maar – de schaalbaarheidsdoelen betrekking hebben op het aantal entiteiten die zijn gescand, zodat een query die veel entiteiten filtert nog steeds tot beperking, leiden kan zelfs als paar entiteiten worden geretourneerd.  

###### <a name="subheading33"></a>Projection
Als u de clienttoepassing slechts een beperkt aantal eigenschappen van de entiteiten in uw tabel moet, kunt u projectie kunt gebruiken om de grootte van de geretourneerde gegevens. Net als bij filteren, dankzij dit minder netwerkbelasting en client verwerken.  

##### <a name="subheading34"></a>Denormalisatie
In tegenstelling tot het werken met relationele databases, wordt de bewezen procedures voor het efficiënt opvragen van tabelgegevens leiden tot denormalizing van uw gegevens. Dat wil zeggen, het dupliceren van dezelfde gegevens in meerdere entiteiten (één voor elke sleutel die u gebruiken kunt om de gegevens te vinden) nodig heeft om het aantal entiteiten waarmee een query moet worden gescand als u wilt de gegevens vinden die de client te beperken, in plaats van dat voor het scannen van grote aantallen entiteiten op de gegevens van uw app vinden groep nodig.  Bijvoorbeeld in een e-commerce-website, kunt u een order beide door de klant-ID zoeken (krijg ik van de klant orders) en op de datum (uitleggen orders op een datum).  In de Table-opslag, het is raadzaam om op te slaan van de entiteit (of een verwijzing naar het) twee keer – één keer door de naam van tabel PK en RK te vergemakkelijken zoeken door de klant-ID, één keer te vergemakkelijken kunt vinden op de datum.  

#### <a name="insertupdatedelete"></a>Invoegen, bijwerken en verwijderen
Deze sectie beschrijft bewezen procedures voor het bewerken van entiteiten die zijn opgeslagen in de table-service.  

##### <a name="subheading35"></a>Batching
Batchtransacties bekend als entiteit groep transacties (ETG) in Azure Storage; alle bewerkingen binnen een ETG moeten zich op een enkele partitie in een enkele tabel. Waar mogelijk, gebruikt u ETGs om uit te voeren invoegen, bijwerken en verwijderen in batches. Dit vermindert het aantal retouren van uw clienttoepassing op de server, vermindert het aantal gefactureerde transacties (een ETG telt als één transactie voor factureringsdoeleinden en mag maximaal 100 opslagbewerkingen) en schakelt atomic updates (alle bewerkingen slagen of mislukken binnen een ETG). Omgevingen met hoge latentie, zoals mobiele apparaten wordt veel voordeel hebben van ETGs.  

##### <a name="subheading36"></a>Upsert
Gebruik tabel **Upsert** bewerkingen mogelijk. Er zijn twee soorten **Upsert**, die beide kunnen efficiënter zijn dan een traditioneel **invoegen** en **Update** bewerkingen:  

* **InsertOrMerge**: Gebruik deze instelling als u wilt uploaden een subset met eigenschappen van de entiteit, maar u niet zeker weet of de entiteit bestaat al. Als de entiteit bestaat, deze aanroep werkt de eigenschappen die zijn opgenomen in de **Upsert** bewerking, en blijven alle bestaande eigenschappen zoals ze zijn, als de entiteit niet bestaat, wordt de nieuwe entiteit ingevoegd. Dit is vergelijkbaar met het gebruik van projectie in een query in dat u alleen hoeft het uploaden van de eigenschappen die zijn gewijzigd.
* **InsertOrReplace**: Gebruik deze instelling als u wilt uploaden, een volledig nieuwe entiteit, maar u niet zeker weet of deze al bestaat. U moet dit alleen gebruiken als u weet dat de zojuist geüploade entiteit helemaal juist is, omdat de oude entiteit volledig overschrijft. Bijvoorbeeld, wilt u bijwerken van de entiteit waarin de huidige locatie van een gebruiker, ongeacht de toepassing wel of niet eerder locatiegegevens voor de gebruiker opgeslagen heeft. de nieuwe locatie-entiteit is voltooid en u hoeft niet gegevens uit een vorige entiteit.

##### <a name="subheading37"></a>Gegevensreeks opslaan in een enkele entiteit
Een toepassing slaat soms, een reeks gegevens die vaak nodig is om op te halen in één keer: bijvoorbeeld een toepassing kan CPU-gebruik gedurende een periode bijhouden om het tekenen van een rolling grafiek van de gegevens van de afgelopen 24 uur. Een aanpak is om één Tabelentiteit per uur, met elke entiteit die een specifiek uur en het CPU-gebruik op te slaan voor dat uur. Als u wilt deze gegevens verwerkt, brengt de toepassing nodig heeft om op te halen van de entiteiten die de gegevens van de 24 uur van de meest recente.  

U kunt ook uw toepassing kan opslaan van het CPU-gebruik voor elk uur als een afzonderlijke eigenschap van een enkele entiteit: uw toepassing kunt voor het bijwerken van elk uur één **InsertOrMerge Upsert** aanroep van de waarde voor de meest recente update uur. Als u wilt de gegevens verwerkt, brengt de toepassing alleen moet één entiteit in plaats van 24 uur per dag, voor een efficiënte query ophalen (Zie hierboven discussie op [query bereik](#subheading30)).

##### <a name="subheading38"></a>Opslaan van gestructureerde gegevens in blobs
Soms gestructureerde gegevens werkt zoals het thuishoort in tabellen, maar bereiken van entiteiten worden altijd samen opgehaald en kan worden batch ingevoegd.  Een goed voorbeeld hiervan is een logboekbestand.  In dit geval kunt u enkele minuten aan logboeken voor batch, voegt u deze en klikt u vervolgens u enkele minuten aan logboeken op een moment ook altijd worden opgehaald.  Voor de prestaties is het in dit geval beter blobs gebruiken in plaats van tabellen, omdat u kunt het aantal objecten geschreven/geretourneerd, evenals gewoonlijk het aantal aanvragen die moeten worden aangebracht aanzienlijk verkorten.  

## <a name="queues"></a>Wachtrijen
Naast de bewezen procedures voor het [alle Services](#allservices) eerder is beschreven, de volgende beproefde procedures name van toepassing op de queue-service.  

### <a name="subheading39"></a>Limieten voor schaalbaarheid
Één wachtrij kunt ongeveer 2000-berichten verwerken (1 KB elke) per seconde (elke AddMessage GetMessage en DeleteMessage tellen als hier een bericht). Als dit niet voldoende voor uw toepassing is moet u meerdere wachtrijen gebruiken en de berichten verdeeld over deze.  

Huidige schaalbaarheidsdoelen op weergeven [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle uitschakelen
Zie de sectie over de configuratie van de tabel die worden beschreven het algoritme Nagle: het algoritme Nagle is in het algemeen niet voor de prestaties van de wachtrij aanvragen en moet u dit uitschakelen.  

### <a name="subheading41"></a>Berichtgrootte
Wachtrij afneemt prestaties en schaalbaarheid wanneer het bericht grootte toeneemt. U moet alleen de gegevens de ontvanger moet in een bericht plaatsen.  

### <a name="subheading42"></a>Het ophalen van batch
U kunt maximaal 32 berichten ophalen uit een wachtrij in één bewerking. Verminder het aantal interactie van de clienttoepassing, dit vooral handig voor omgevingen, zoals mobiele apparaten is, kan dit met een hoge latentie.  

### <a name="subheading43"></a>Polling-Interval van wachtrij
De meeste toepassingen of er berichten uit een wachtrij, dit kan een van de grootste bronnen van transacties voor de toepassing. Selecteer de polling-interval goed te overwegen: te vaak polling kan ervoor zorgen dat uw toepassing om de schaalbaarheidsdoelen voor de wachtrij. Op 200.000 transacties voor $0,01 (op het moment van schrijven) is één processor polling eenmaal per seconde voor een maand zou u minder dan 15 cent, dus kosten echter niet normaal gesproken een factor die van invloed is op uw eigen keuze aan polling-interval.  

Zie voor meer kosteninformatie recente, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
U kunt **UpdateMessage** te verhogen van de time-out voor onzichtbaarheid of bijwerken van informatie over de status van een bericht. Dit is een krachtige, houd er rekening mee dat elke **UpdateMessage** bewerking wordt geteld tegen het schaalbaarheidsdoel. Dit kan echter zijn dat een benadering veel efficiënter dan een werkstroom waarmee een taak uit een wachtrij worden doorgegeven aan de volgende, om elke stap van de taak is voltooid. Met behulp van de **UpdateMessage** bewerking kan uw toepassing voor het opslaan van de taakstatus van de op het bericht en vervolgens doorgaan met werken, in plaats van het bericht voor de volgende stap van de taak opnieuw queuing telkens wanneer een stap is voltooid.  

Zie voor meer informatie het artikel [het: Wijzig de inhoud van een bericht in de wachtrij](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Toepassingsarchitectuur
U moet wachtrijen gebruiken om de toepassingsarchitectuur van uw schaalbaar maken. Hieronder vindt u enkele manieren waarop u kunt wachtrijen gebruiken om uw toepassingen beter schaalbaar:  

* U kunt wachtrijen gebruiken voor het maken van achterstanden voor verwerking en gebruiken om de werkbelasting in uw toepassing. U kunt bijvoorbeeld wachtrij komen aanvragen van gebruikers om uit te voeren van de processor intensief werk zoals het formaat van geüploade afbeeldingen.
* U kunt wachtrijen gebruiken loskoppelen van onderdelen van uw toepassing zodat u ze onafhankelijk kunt schalen. Resultaten van het onderzoek van gebruikers kan bijvoorbeeld plaatsen, een webfront-end in een wachtrij voor latere analyse en opslag. U kunt meer instanties van de werkrol voor het verwerken van de wachtrijgegevens naar behoefte kunt toevoegen.  

## <a name="conclusion"></a>Conclusie
In dit artikel besproken enkele van de meest voorkomende en bewezen procedures voor het optimaliseren van prestaties bij het gebruik van Azure Storage. Wij raden elke toepassingsontwikkelaar aan om toepassingen te beoordelen aan de hand van elk van de bovenstaande procedures en de aanbevelingen te overwegen voor het verkrijgen van uitstekende prestaties voor toepassingen die gebruikmaken van Azure Storage.
