---
title: Azure Storage-controlelijst voor prestaties en schaalbaarheid | Microsoft Docs
description: Een controlelijst met bewezen voor gebruik met Azure Storage zodat toepassingen te ontwikkelen.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 959d831b-a4fd-4634-a646-0d2c0c462ef8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 6f5a136d1be7a4bb4093baad820271770305b718
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-storage-performance-and-scalability-checklist"></a>Controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage
## <a name="overview"></a>Overzicht
Microsoft heeft een aantal beproefde procedures voor het gebruik van deze services op een manier zodat ontwikkeld sinds de release van de Microsoft Azure Storage-services en in dit artikel fungeert de belangrijkste hiervan consolideren in een controlelijst-stijl-lijst. De bedoeling van dit artikel is om te controleren of dat ze bewezen gebruiken met Azure Storage toepassingsontwikkelaars en zodat andere bewezen overstap overweegt identificeren. In dit artikel probeert niet elke mogelijke optimalisatie van prestaties en schaalbaarheid omvatten, worden uitgesloten die gevolgen klein of niet algemeen van toepassing zijn. Voor zover dit gedrag van de toepassing kan worden voorspeld tijdens het ontwerpen, is het nuttig te houden in gedachten vroeg stadium te ontwerpen die wordt uitgevoerd in prestatieproblemen te voorkomen.  

Elke ontwikkelaar van toepassingen met behulp van Azure Storage moet even de tijd in dit artikel lezen en controleren van toepassing zijn of haar volgt elk van de hieronder vermelde bewezen.  

## <a name="checklist"></a>Controlelijst
In dit artikel worden de bewezen in de volgende groepen ingedeeld. Bewezen toepassingen die van toepassing op:  

* Alle Azure Storage-services (blobs, tabellen, wachtrijen en -bestanden)
* Blobs
* Tabellen
* Wachtrijen  

| Klaar | Onderwerp | Category | Vraag |
| --- | --- | --- | --- |
| &nbsp; | Alle services |Schaalbaarheidsdoelen |[Is uw toepassing ontworpen om te voorkomen dat de schaalbaarheidsdoelen bijna?](#subheading1) |
| &nbsp; | Alle services |Schaalbaarheidsdoelen |[Is uw naamgevingsconventie ontworpen om in te schakelen beter taakverdeling?](#subheading47) |
| &nbsp; | Alle services |Netwerken |[Hebt side clientapparaten hoog genoeg bandbreedte en lage latentie voor de prestaties die nodig zijn?](#subheading2) |
| &nbsp; | Alle services |Netwerken |[Hebt clientapparaten naast een koppeling hoog genoeg kwaliteit?](#subheading3) |
| &nbsp; | Alle services |Netwerken |[De clienttoepassing vindt 'aan' het storage-account?](#subheading4) |
| &nbsp; | Alle services |Inhoudsdistributie |[Gebruikt u de CDN voor distributie van inhoud?](#subheading5) |
| &nbsp; | Alle services |Directe clienttoegang |[Gebruikt u SAS en CORS voor directe toegang tot opslag in plaats van proxy toestaan?](#subheading6) |
| &nbsp; | Alle services |Caching |[Uw cache toepassingsgegevens die herhaaldelijk wordt gebruikt en de wijzigingen zelden is?](#subheading7) |
| &nbsp; | Alle services |Caching |[Is uw toepassing batchverwerking updates (deze clientzijde in cache en vervolgens uploaden in grotere sets)?](#subheading8) |
| &nbsp; | Alle services |.NET-configuratie |[Hebt u de client voor het gebruik van een voldoende aantal gelijktijdige verbindingen geconfigureerd?](#subheading9) |
| &nbsp; | Alle services |.NET-configuratie |[Hebt u .NET voor het gebruik van een voldoende aantal threads geconfigureerd?](#subheading10) |
| &nbsp; | Alle services |.NET-configuratie |[Gebruikt u de .NET 4.5 of hoger, waarop garbagecollection is verbeterd?](#subheading11) |
| &nbsp; | Alle services |Parallelle uitvoering |[U zorgt dat parallelle uitvoering op de juiste wijze wordt begrensd zodat u niet de clientmogelijkheden van de of de schaalbaarheidsdoelen van de overbelasting?](#subheading12) |
| &nbsp; | Alle services |Hulpprogramma's |[Vindt u de nieuwste versie van Microsoft clientbibliotheken en hulpprogramma's?](#subheading13) |
| &nbsp; | Alle services |Nieuwe pogingen |[Weet u exponentieel uitstel met beleid voor beperking van de fouten en time-outs voor opnieuw proberen?](#subheading14) |
| &nbsp; | Alle services |Nieuwe pogingen |[Is uw toepassing waarbij pogingen voor niet-herstelbare fouten?](#subheading15) |
| &nbsp; | Blobs |Schaalbaarheidsdoelen |[Hebt u een groot aantal clients tegelijkertijd toegang krijgen tot een enkel object?](#subheading46) |
| &nbsp; | Blobs |Schaalbaarheidsdoelen |[Is uw toepassing binnen het doel van de schaalbaarheid bandbreedte of bewerkingen voor één blob blijft?](#subheading16) |
| &nbsp; | Blobs |Blobs kopiëren |[Weet u kopiëren blobs op efficiënte wijze?](#subheading17) |
| &nbsp; | Blobs |Blobs kopiëren |[Gebruikt u AzCopy voor bulksgewijs kopieën van BLOB's?](#subheading18) |
| &nbsp; | Blobs |Blobs kopiëren |[Gebruikt u Azure Import/Export om over te dragen van zeer grote hoeveelheden gegevens?](#subheading19) |
| &nbsp; | Blobs |Met behulp van metagegevens |[Bewaart u veelgebruikte metagegevens over blobs in de metagegevens?](#subheading20) |
| &nbsp; | Blobs |Snel uploaden |[Bij het snel uploaden van een blob, uploadt u blokken parallel?](#subheading21) |
| &nbsp; | Blobs |Snel uploaden |[Wanneer u probeert veel blobs snel uploaden, uploadt u blobs parallel?](#subheading22) |
| &nbsp; | Blobs |Juiste Blob-Type |[Gebruikt u pagina-blobs of blok-blobs als het nodig?](#subheading23) |
| &nbsp; | Tabellen |Schaalbaarheidsdoelen |[Bent u bijna de schaalbaarheidsdoelen voor entiteiten per seconde?](#subheading24) |
| &nbsp; | Tabellen |Configuratie |[Gebruikt u JSON voor uw tabel aanvragen?](#subheading25) |
| &nbsp; | Tabellen |Configuratie |[Hebt u uitgeschakeld Nagle verbeteren de prestaties van kleine aanvragen?](#subheading26) |
| &nbsp; | Tabellen |Tabellen en partities |[Uw gegevens correct gepartitioneerd?](#subheading27) |
| &nbsp; | Tabellen |Hot partities |[Weet u alleen toevoegen en voeg alleen-lezen patronen voorkomen?](#subheading28) |
| &nbsp; | Tabellen |Hot partities |[Worden uw bijgewerkte voegt verdeeld over veel partities?](#subheading29) |
| &nbsp; | Tabellen |Querybereik |[Hebt u uw schema om toe te staan voor punt-query's moet worden gebruikt in de meeste gevallen en tabel query's moeten spaarzaam worden gebruikt die is ontworpen?](#subheading30) |
| &nbsp; | Tabellen |Query-dichtheid |[Uw query's doorgaans alleen scan doen en retourneert de rijen die door uw toepassing wordt gebruikt?](#subheading31) |
| &nbsp; | Tabellen |Beperkende geretourneerde gegevens |[Gebruikt u voor het filteren om te voorkomen dat er geretourneerd entiteiten die niet nodig zijn?](#subheading32) |
| &nbsp; | Tabellen |Beperkende geretourneerde gegevens |[Gebruikt u projectie voorkomen dat er geretourneerd eigenschappen die niet nodig zijn?](#subheading33) |
| &nbsp; | Tabellen |Denormalization |[Hebt u uw gegevens gedenormaliseerd zodat u inefficiënte query's of meerdere leesaanvragen voorkomen bij het ophalen van gegevens?](#subheading34) |
| &nbsp; | Tabellen |Invoegen, bijwerken, verwijderen |[Weet u batchverwerking aanvragen die moeten worden transactionele of op hetzelfde moment te verminderen retourbewerkingen kan worden uitgevoerd?](#subheading35) |
| &nbsp; | Tabellen |Invoegen, bijwerken, verwijderen |[Weet u vermijden bij het ophalen van een entiteit die u zojuist hebt om te bepalen of aan te roepen invoegen of bijwerken?](#subheading36) |
| &nbsp; | Tabellen |Invoegen, bijwerken, verwijderen |[Hebt u beschouwd als reeks gegevens dat wordt vaak opgehaald samen in één entiteit opslaan als eigenschappen in plaats van meerdere entiteiten?](#subheading37) |
| &nbsp; | Tabellen |Invoegen, bijwerken, verwijderen |[Voor entiteiten die wordt altijd samen worden opgehaald en kunnen worden geschreven in batches (bijvoorbeeld tijd reeksgegevens), hebt u beschouwd als blobs in plaats van tabellen gebruiken?](#subheading38) |
| &nbsp; | Wachtrijen |Schaalbaarheidsdoelen |[Bent u bijna de schaalbaarheidsdoelen voor berichten die per seconde?](#subheading39) |
| &nbsp; | Wachtrijen |Configuratie |[Hebt u uitgeschakeld Nagle verbeteren de prestaties van kleine aanvragen?](#subheading40) |
| &nbsp; | Wachtrijen |Berichtgrootte |[Uw berichten compact verbeteren de prestaties van de wachtrij zijn?](#subheading41) |
| &nbsp; | Wachtrijen |Bulksgewijs ophalen |[Weet u bij het ophalen van meerdere berichten in één bewerking 'Ophalen'?](#subheading42) |
| &nbsp; | Wachtrijen |Pollingfrequentie opgeven |[Weet u polling vaak genoeg om te beperken van de merkbare latentie van uw toepassing?](#subheading43) |
| &nbsp; | Wachtrijen |Updatebericht |[Gebruikt u UpdateMessage voor het opslaan van voortgang in verwerking van berichten, waardoor wordt voorkomen dat het volledige bericht opnieuw verwerken als een fout optreedt?](#subheading44) |
| &nbsp; | Wachtrijen |Architectuur |[Gebruikt u wachtrijen ervoor dat uw hele toepassing meer schaalbare doordat langlopende werkbelastingen buiten het kritieke pad en vervolgens afzonderlijk van elkaar schalen?](#subheading45) |

## <a name="allservices"></a>Alle Services
Deze sectie vindt bewezen toepassingen die van toepassing op het gebruik van een van de Azure Storage-services (blobs, tabellen, wachtrijen of bestanden).  

### <a name="subheading1"></a>Schaalbaarheidsdoelen
Elk van de Azure Storage-services heeft schaalbaarheidsdoelen voor capaciteit (GB), Transactiesnelheid en bandbreedte. Als uw toepassing nadert of hoger is dan een van de schaalbaarheidsdoelen, ondervinden het toegenomen transactie latenties of beperking. Wanneer een opslagservice uw toepassing bandbreedte, begint de service '503 Server bezet' of '500 time-out voor de bewerking' foutcodes voor sommige opslagtransacties als resultaat geven. Deze sectie wordt de algemene benadering voor het omgaan met schaalbaarheidsdoelen en schaalbaarheidsdoelen van bandbreedte, met name besproken. Latere secties die betrekking op afzonderlijke opslagservices hebben behandeld schaalbaarheidsdoelen in de context van die service:  

* [BLOB-bandbreedte en aanvragen per seconde](#subheading16)
* [Tabelentiteiten per seconde](#subheading24)
* [Wachtrijberichten per seconde](#subheading39)  

#### <a name="sub1bandwidth"></a>Doel van schaalbaarheid bandbreedte voor alle Services
Op het moment van schrijven zijn de doelen van de bandbreedte in de Verenigde Staten voor een account met geografisch redundante opslag (GRS) 10 Gigabit per seconde (Gbps) voor inkomende (gegevens verzonden naar het opslagaccount) en 20 Gbps voor uitgaande (gegevens verzonden van het storage-account). Voor een account lokaal redundante opslag (LRS) de grenzen zijn hogere – 20 Gbps voor inkomend en 30 Gbps voor uitgaande.  Internationale bandbreedtelimieten mogelijk lager en vindt u op onze [schaalbaarheid van de pagina](http://msdn.microsoft.com/library/azure/dn249410.aspx).  Zie voor meer informatie over de opties voor opslag redundantie de koppelingen in [nuttige informatiebronnen](#sub1useful) hieronder.  

#### <a name="what-to-do-when-approaching-a-scalability-target"></a>Wat te doen wanneer een doel schaalbaarheid nadert
Als uw toepassing uitgevoerd, de schaalbaarheidsdoelen voor een enkele storage-account bereikt wordt, kunt u de overstap op een van de volgende methoden:  

* Andere de werkbelasting die ervoor zorgt uw toepassing dat te benaderen of hoger zijn dan het doel van de schaalbaarheid. Kunt u ook anders minder bandbreedte of capaciteit of minder transacties gebruiken ontwerpen?
* Als een toepassing moet groter zijn dan een van de schaalbaarheidsdoelen, moet u meerdere opslagaccounts en partitie uw toepassingsgegevens tussen die meerdere opslagaccounts. Als u dit patroon gebruikt, vervolgens moet u voor het ontwerpen van uw toepassing zodat u meer storage-accounts in de toekomst voor taakverdeling kunt toevoegen. Op het moment van schrijven, kan elk Azure-abonnement maximaal 100 opslagaccounts hebben.  Storage-accounts hebben ook geen kosten dan gebruik van uw in termen van de gegevens die zijn opgeslagen, transacties die zijn aangebracht of gegevens die zijn overgebracht.
* Als uw toepassing de doelen van bandbreedte raakt, kunt u overwegen het comprimeren van gegevens in de client en het verminderen van de bandbreedte die de gegevens verzenden naar de storage-service is vereist.  Houd er rekening mee dat terwijl dit kunt u bandbreedte besparen en verbetering van netwerkprestaties, het kan ook sommige negatieve gevolgen hebben.  De prestatie-invloed van deze vanwege de vereisten voor extra verwerking voor het comprimeren en decomprimeren van gegevens in de client, moet u evalueren. Bovendien kan opslaan van gecomprimeerde gegevens het moeilijker problemen kunt oplossen, omdat deze mogelijk moeilijker om opgeslagen gegevens met standaardprogramma's weer te geven.
* Als uw toepassing de schaalbaarheidsdoelen raakt, Verzeker u ervan dat u van exponentieel uitstel voor nieuwe pogingen gebruikmaakt (Zie [pogingen](#subheading14)).  Is het beter om ervoor te zorgen nooit benadering van de schaalbaarheidsdoelen (met behulp van een van de bovenstaande methoden), maar hierdoor zorgt u dat uw toepassing won't NET blijft proberen snel, waardoor de beperking slechter.  

#### <a name="useful-resources"></a>Nuttige informatie
De volgende koppelingen bieden aanvullende details voor schaalbaarheidsdoelen:

* Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie over schaalbaarheidsdoelen.
* Zie [Azure Storage-replicatie](storage-redundancy.md) en de blogpublicatie [opslagopties van Azure voor redundantie en geografisch redundante opslag met leestoegang](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) voor meer informatie over opties voor opslag redundantie.
* Zie voor actuele informatie over prijzen voor Azure-services [Azure-prijzen](https://azure.microsoft.com/pricing/overview/).  

### <a name="subheading47"></a>De naamconventie partitie
Azure-opslag maakt gebruik van een bereik gebaseerde partitieschema schaal en load toepassen van het systeem. De partitiesleutel wordt gebruikt voor de partitie gegevens in de bereiken en deze bereiken zijn Netwerktaakverdeling in het systeem. Dit betekent dat naamconventies zoals lexicale ordenen (bijvoorbeeld msftpayroll, msftperformance, msftemployees, enzovoort) of met behulp van tijdstempels (log20160101, log20160102, log20160102, enzovoort) wordt zichzelf goed lenen voor de partities wordt mogelijk CO-locatie op de server met dezelfde partitie, tot aan een load balancing bewerking splitst deze in kleinere bereiken. Alle blobs in een container kunnen bijvoorbeeld worden geleverd door één server, totdat de belasting van deze blobs vereist meer herverdeling van de partitie bereiken. Op dezelfde manier een groep licht geladen accounts met hun namen lexicale volgorde kan worden geleverd door een enkele server totdat de belasting op één of al deze accounts moet u ze om te worden verdeeld over meerdere partities servers. Elke load balancer bewerking mogelijk van invloed op de latentie van storage aanroepen tijdens de bewerking. Mogelijkheid voor het afhandelen van een plotselinge toename van het verkeer naar een partitie van het systeem wordt beperkt door de schaalbaarheid van een server met één partitie totdat de bewerking voor taakverdeling in gang en de partitiesleutelbereik rebalances.  

U kunt volgen enkele aanbevolen procedures voor het Verlaag de frequentie van dergelijke bewerkingen.  

* Bekijk de naamgevingsconventie die u voor accounts, containers, blobs, tabellen en wachtrijen, nauw gebruikt. U kunt de accountnamen Mining met een 3-cijferige hash met een hash-functie die het beste past bij uw behoeften.  
* Als u uw gegevens dankzij tijdstempels of numerieke id's organiseren, hebt u ervoor dat u niet gebruikmaken van een verkeerspatronen alleen toevoegen (of alleen-toevoegen). Deze patronen zijn niet geschikt voor een bereik-gebaseerd systeem partitioneren en kan leiden tot alle verkeer van één partitie en effectief beperken van het systeem van taakverdeling. Bijvoorbeeld als u de dagelijkse bewerkingen die gebruikmaken van een blob-object met een tijdstempel zoals JJJJMMDD hebt, is klikt u vervolgens het verkeer voor de dagelijkse bewerking gericht op een enkel object die worden geleverd door een server met één partitie. Bekijk of de per limieten voor blob per partitie limieten aan uw behoeften voldoet en u kunt deze bewerking in meerdere blobs op te splitsen indien nodig. Op dezelfde manier als u een reeksgegevens in de tabellen opslaat, al het verkeer kan worden omgeleid naar het laatste deel van de belangrijkste naamruimte. Als u tijdstempels of numerieke id's moet gebruiken, het voorvoegsel van de id met een 3-cijferige hash of in het geval van tijdstempels het seconden deel van de tijd zoals ssyyyymmdd voorvoegsel. Als het weergeven en opvragen bewerkingen worden regelmatig uitgevoerd, kiest u een hash-functie die wordt beperkt het aantal query's. In andere gevallen mogelijk een willekeurige voorvoegsel voldoende.  
* Voor meer informatie over het partitieschema gebruikt in Azure Storage Lees het artikel SOSP [hier](http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf).

### <a name="networking"></a>Netwerken
Terwijl de API materie aanroept, hebben de beperkingen van het fysieke netwerk van de toepassing vaak een aanzienlijke invloed op de prestaties. Het volgende worden een aantal beperkingen die gebruikers kunnen ondervinden.  

#### <a name="client-network-capability"></a>Mogelijkheid via het netwerk van de client
##### <a name="subheading2"></a>Doorvoer
Voor bandbreedte is het probleem vaak de mogelijkheden van de client. Bijvoorbeeld, terwijl een enkele opslagaccount 10 Gbps of meer van de inkomende gegevens kan verwerken (Zie [schaalbaarheidsdoelen van bandbreedte](#sub1bandwidth)), de netwerksnelheid in een exemplaar van 'Kleine' Azure Worker-rol is alleen geschikt voor ongeveer 100 Mbps. Grotere Azure-exemplaren hebben NIC's met grotere capaciteit, dus u overwegen moet een grotere exemplaar of meer van de virtuele machine als u een hogere netwerk limieten vanaf één computer nodig. Als u een service Storage vanuit een on-premises-toepassing opent, wordt dezelfde regel van toepassing is: inzicht in de netwerkmogelijkheden van het clientapparaat en de netwerkverbinding met de Azure-opslaglocatie en u ze als nodig is of ontwerpen van uw toepassing uit te voeren binnen hun mogelijkheden verbeteren.  

##### <a name="subheading3"></a>Kwaliteit van de verbinding
Net als bij alle netwerkgebruik Bedenk dat netwerkomstandigheden, wat leidt tot fouten en pakketverlies effectieve doorvoer vertragen.  Met behulp van WireShark of NetMon kan helpen bij het oplossen van dit probleem.  

##### <a name="useful-resources"></a>Nuttige informatie
Zie voor meer informatie over grootten van virtuele machines en toegewezen bandbreedte [Windows VM-grootten](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) of [Linux VM-grootten](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

#### <a name="subheading4"></a>Locatie
In een gedistribueerde omgeving biedt plaatsen van de client in de buurt bevindt de server in de beste prestaties. Voor toegang tot Azure Storage met de laagste latentie, is de aanbevolen locatie voor de client binnen dezelfde Azure-regio. Als u een Azure-website die gebruikmaakt van Azure Storage hebt, moet u bijvoorbeeld beide in één regio (bijvoorbeeld VS-West of Zuidoost-Azië). Dit vermindert de latentie en de kosten, op het moment van schrijven, bandbreedtegebruik binnen één regio is gratis.  

Als uw toepassingen niet in Azure (zoals apps voor mobiele apparaten of op de lokale enterprise-services), klikt u vervolgens opnieuw gehost worden client plaatsen van het storage-account in een regio in de buurt bevindt de apparaten die toegang hebben tot, in het algemeen de latentie te verminderen wordt. Als uw clients worden grote schaal gedistribueerd (voor bijvoorbeeld enkele in Noord-Amerika, en sommige in Europa), dan moet u overwegen meerdere opslagaccounts: een zich bevinden in een regio Noord-Amerika en een in een Europese regio. Dit helpt de latentie voor gebruikers in beide regio's te verminderen. Deze aanpak is meestal eenvoudiger te implementeren als de gegevens door de toepassing opgeslagen is specifiek voor afzonderlijke gebruikers en vereist niet dat voor het repliceren van gegevens tussen opslagaccounts.  Voor een brede distributie van inhoud, verdient het aanbeveling een CDN: Zie de volgende sectie voor meer informatie.  

### <a name="subheading5"></a>Distributie van inhoud
Soms moet een toepassing voor dezelfde inhoud aan meerdere gebruikers (bijvoorbeeld een product demovideo gebruikt in de startpagina van een website), zich in dezelfde of verschillende regio's. In dit scenario moet u een Content Delivery Network (CDN) zoals Azure CDN gebruiken en de CDN Azure-opslag wilt gebruiken als de oorsprong van de gegevens. In tegenstelling tot een Azure-opslagaccount die zich in één regio en die inhoud niet afleveren bij een lage latentie naar andere regio's, gebruikt Azure CDN servers in meerdere datacenters over de hele wereld. Bovendien kan een CDN doorgaans veel hoger uitgaande limieten dan een enkele opslagaccount ondersteunen.  

Zie voor meer informatie over Azure CDN [Azure CDN](https://azure.microsoft.com/services/cdn/).  

### <a name="subheading6"></a>Met behulp van SAS- en CORS
Als u toestaan, zoals JavaScript-code in de webbrowser van een gebruiker of een mobiele telefoon-app voor toegang tot gegevens in Azure Storage wilt, een aanpak is voor het gebruik van een toepassing in Webrol als proxy: apparaat van de gebruiker wordt geverifieerd met de Webrol, op zijn beurt is geverifieerd met de storage-service. Op deze manier kunt u voorkomen dat blootstellen van uw toegangscodes voor opslag op onbeveiligde apparaten. Dit wordt een groot overhead echter geplaatst op de Webrol omdat alle gegevens die worden overgedragen tussen apparaat van de gebruiker en de storage-service aan de Webrol doorgeven moet. U kunt het gebruik van een Webrol als proxy voor de storage-service met behulp van de Shared Access Signatures (SAS), soms in combinatie met koppen Cross-Origin-Resource delen (CORS) voorkomen. Met SAS, kunt u uw gebruikersapparaat aanvragen rechtstreeks naar een opslagservice maken met behulp van een token met beperkte toegang toestaan. Bijvoorbeeld, als een gebruiker wil een foto uploadt naar uw toepassing, kunt de Webrol genereren en verzenden naar apparaat van de gebruiker een SAS-token dat toegang biedt tot het schrijven naar een specifieke blob of een container voor de komende 30 minuten (waarna het SAS-token vervalt).

Een browser wordt normaal gesproken niet JavaScript toestaan op een pagina die wordt gehost door een website op één domein bepaalde bewerkingen, zoals een 'PUT' naar een ander domein uit te voeren. Bijvoorbeeld, als u een Webrol op 'contosomarketing.cloudapp.net' host, en wilt gebruiken van client-side JavaScript een blob uploaden naar uw opslagaccount op 'contosoproducts.blob.core.windows.net,' de browser van 'dezelfde oorsprong beleid' deze bewerking wordt verbieden. CORS is een browser-functie waarmee het doeldomein (in dit geval het storage-account) om te communiceren met de browser dat aanvragen die afkomstig zijn in het brondomein (in dit geval de Webrol) wordt vertrouwd.  

Beide technologieën kunt u onnodige laden (en knelpunten) op uw webtoepassing voorkomen.  

#### <a name="useful-resources"></a>Nuttige informatie
Zie voor meer informatie over SAS [handtekeningen voor gedeelde toegang, deel 1: inzicht in het SAS-Model](../storage-dotnet-shared-access-signature-part-1.md).  

Zie voor meer informatie over CORS [Cross-Origin-Resource delen (CORS) ondersteuning voor de Azure Storage-Services](http://msdn.microsoft.com/library/azure/dn535601.aspx).  

### <a name="caching"></a>Caching
#### <a name="subheading7"></a>Ophalen van gegevens
In het algemeen is het beter dan tweemaal aan eenmaal ophalen van gegevens van een service. Bekijk het voorbeeld van een MVC-webtoepassing uitgevoerd in een Webrol die heeft al een blob 50MB opgehaald uit de storage-service om te fungeren als de inhoud naar een gebruiker. De toepassing kan vervolgens dezelfde blob ophalen telkens wanneer een gebruiker vraagt of het cache plaatsen kan lokaal naar schijf en de versie van de cache voor daaropvolgende gebruikersaanvragen hergebruiken. Bovendien telkens wanneer een gebruiker vraagt de gegevens, de toepassing kan probleem verkrijgen met een voorwaardelijke kop voor wijzigingstijd voorkomen de hele blob dat zou als deze niet is gewijzigd. U kunt dit patroon toepassen op het werken met tabelentiteiten.  

In sommige gevallen kunt u beslissen dat uw toepassing wordt ervan uitgegaan dat de blob geldig blijft gedurende een korte periode na het en dat gedurende deze periode de toepassing niet hoeft te controleren of de blob is gewijzigd.

Configuratie, opzoeken en andere gegevens die altijd door de toepassing gebruikt worden zijn goede kandidaten voor opslaan in cache.  

Zie voor een voorbeeld van hoe u een blob-eigenschappen voor het detecteren van de datum van laatste wijziging met .NET [Set eigenschappen ophalen en metagegevens en](../blobs/storage-properties-metadata.md). Zie voor meer informatie over voorwaardelijke downloads [voorwaardelijk vernieuwen van een lokale kopie van een Blob](http://msdn.microsoft.com/library/azure/dd179371.aspx).  

#### <a name="subheading8"></a>Uploaden van gegevens in Batches
In bepaalde situaties toepassing, kunt u statistische gegevens lokaal en periodiek uploaden in een batch in plaats van elk gegevensitem direct te uploaden. Bijvoorbeeld, een webtoepassing ervoor kiezen om een logbestand van activiteiten: de toepassing kan details van elke activiteit uploadt als dit als een Tabelentiteit gebeurt (waarvoor veel opslagbewerkingen) of het bestand kan activiteitendetails in een lokale logboekbestand opslaan en alle details van computeractiviteit periodiek als een bestand met scheidingstekens naar een blob te uploaden. Als elke logboekvermelding 1KB groot is, kunt u duizenden in één 'Blob plaatsen' transactie (u kunt een blob van maximaal 64MB groot in één transactie uploaden) uploaden. Natuurlijk als de lokale computer voordat het uploaden vastloopt, wordt mogelijk verbroken sommige logboekgegevens: ontwikkelaar van de toepassing moet de mogelijkheid van clientapparaat ontwerpt of fouten uploaden.  Als de activiteitsgegevens moet worden gedownload voor timespans (niet slechts één activiteit), worden de blobs voorkeur boven tabellen.

### <a name="net-configuration"></a>.NET-configuratie
Deze sectie vindt u enkele snelle configuratie-instellingen die u gebruiken kunt voor het maken van belangrijke prestatieverbeteringen als u gebruikmaakt van .NET Framework.  Als u andere talen, controleert u of vergelijkbare concepten toepassing in uw gekozen taal.  

#### <a name="subheading9"></a>Standaard verbindingslimiet verhogen
De volgende code verhoogt in .NET de standaard verbindingslimiet bereikt (dit is doorgaans 2 in de clientomgeving van een of 10 in een serveromgeving) en 100. Normaal gesproken moet u de waarde ingesteld op ongeveer het aantal threads die worden gebruikt door de toepassing.  

```csharp
ServicePointManager.DefaultConnectionLimit = 100; //(Or More)  
```

U moet de verbindingslimiet voor het openen van alle verbindingen instellen.  

Zie de documentatie voor die taal te bepalen hoe de verbindingslimiet instellen voor andere programmeertalen.  

Zie voor meer informatie het blogbericht [webservices: gelijktijdige verbindingen](http://blogs.msdn.com/b/darrenj/archive/2005/03/07/386655.aspx).  

#### <a name="subheading10"></a>ThreadPool Min Threads te verhogen als synchrone code met asynchrone taken
Deze code wordt de thread pool min-threads te verhogen:  

```csharp
ThreadPool.SetMinThreads(100,100); //(Determine the right number for your application)  
```

Zie voor meer informatie [ThreadPool.SetMinThreads methode](http://msdn.microsoft.com/library/system.threading.threadpool.setminthreads%28v=vs.110%29.aspx).  

#### <a name="subheading11"></a>Profiteren van de .NET 4.5-garbagecollection
.NET 4.5 of hoger voor de clienttoepassing gebruiken om te profiteren van betere prestaties in server garbagecollection.

Zie voor meer informatie het artikel [een overzicht van prestatieverbeteringen in .NET 4.5](http://msdn.microsoft.com/magazine/hh882452.aspx).  

### <a name="subheading12"></a>Ongebonden parallelle uitvoering
Parallelle uitvoering kan zijn ideaal voor prestaties, Wees voorzichtig bij gebruik van ongebonden parallelle uitvoering (geen limiet voor het aantal threads en/of parallelle aanvragen) om te uploaden of downloaden van gegevens, met meerdere werknemers voor toegang tot meerdere partities (containers, wachtrijen of Tabelpartities) in hetzelfde opslagaccount of toegang krijgen tot meerdere items in dezelfde partitie. Als de parallelle uitvoering unbounded, uw toepassing kan groter zijn dan het clientapparaat mogelijkheden of schaalbaarheid van het opslagaccount is bedoeld voor wat resulteert in langere latentietijden en beperking.  

### <a name="subheading13"></a>Opslagclientbibliotheken en hulpprogramma 's
Gebruik altijd de meest recente Microsoft opgegeven clientbibliotheken en hulpprogramma's. Op het moment van schrijven zijn beschikbaar voor .NET, Windows Phone, Windows Runtime, Java en C++ clientbibliotheken, evenals de preview-bibliotheken voor andere talen. Microsoft heeft bovendien uitgebracht PowerShell-cmdlets en Azure CLI-opdrachten voor het werken met Azure Storage. Microsoft actief deze hulpprogramma's met het oog op prestaties ontwikkelt, up-to-date met de nieuwste Serviceversies blijft en zorgt ervoor dat ze beproefde prestaties praktijken van intern verwerkt.  

### <a name="retries"></a>Nieuwe pogingen
#### <a name="subheading14"></a>Beperking/ServerBusy
In sommige gevallen kan de storage-service kan uw toepassing beperken of gewoon mogelijk geen dient voor de aanvraag als gevolg van een aantal tijdelijke voorwaarde en een bericht '503 Server bezet' of '500 time-out' retourneren.  Dit kan gebeuren als uw toepassing bijna de van schaalbaarheidsdoelen is bereikt, of als het systeem is herverdeling uw gepartitioneerde gegevens om toe te staan voor een hogere doorvoer.  De clienttoepassing moet doorgaans probeer het opnieuw dat ervoor zorgt een fout opgetreden dat: probeert dezelfde aanvraag later kan worden voltooid. Als de storage-service is uw toepassing beperking, omdat deze schaalbaarheidsdoelen overschrijdt, of zelfs als de service heeft geen voor de aanvraag om een andere reden, nog agressieve pogingen meestal wel de probleem verergert. Daarom moet u een exponentiële uit (de client-bibliotheken standaard dit gedrag). Uw toepassing kan bijvoorbeeld opnieuw proberen na twee seconden vervolgens 4 seconden vervolgens 10 seconden vervolgens 30 seconden en vervolgens gaven ze volledig. Dit gedrag resulteert in uw toepassing aanzienlijk verminderen van de belasting van de service in plaats van exacerbating problemen.  

Houd er rekening mee dat connectiviteitsfouten kunnen opnieuw worden geprobeerd onmiddellijk, omdat ze niet het resultaat van de beperking van en naar verwachting wordt tijdelijke.  

#### <a name="subheading15"></a>Niet-herstelbare fouten
De clientbibliotheken zich bewust zijn van welke fouten opnieuw kunnen zijn en welke niet. Echter als u uw eigen code te vergelijken met de REST-API van de opslag schrijft, moet u er zijn enkele fouten die u niet proberen: bijvoorbeeld 400 (ongeldige aanvraag) antwoord geeft aan dat de clienttoepassing een aanvraag kan niet worden verwerkt omdat deze niet in een verwachte formulier verzonden. Deze aanvraag opnieuw te verzenden leidt tot hetzelfde antwoord telkens, zodat er geen punt in het opnieuw proberen. Als u uw eigen code te vergelijken met de REST-API van de opslag schrijft, worden op de hoogte van de betekenis van de foutcodes en de juiste wijze opnieuw proberen (of niet) voor elk van deze.  

#### <a name="useful-resources"></a>Nuttige informatie
Zie voor meer informatie over foutcodes opslag [Status en foutcodes](http://msdn.microsoft.com/library/azure/dd179382.aspx) op de website van Microsoft Azure.  

## <a name="blobs"></a>Blobs
Naast de bewezen [alle Services](#allservices) hierboven beschreven, de volgende procedures bewezen name van toepassing op de blob-service.  

### <a name="blob-specific-scalability-targets"></a>Van de BLOB-specifieke Schaalbaarheidsdoelen
#### <a name="subheading46"></a>Meerdere clients tegelijkertijd toegang krijgen tot een enkel object
Als u hebt een groot aantal clients tegelijkertijd toegang krijgen tot een enkel object moet u rekening moet houden per object en opslag schaalbaarheidsdoelen van het account. Het exacte aantal clients die toegang heeft tot een enkel object zal variëren afhankelijk van factoren zoals het aantal clients tegelijk, aanvragen van het object de grootte van het object, netwerk-voorwaarden enzovoort.

Als het object kan worden gedistribueerd via de CDN bijvoorbeeld installatiekopieën of video's geleverd vanuit een website en moet u een CDN. Zie [hier](#subheading5).

In andere scenario's zoals wetenschappelijke simulaties waar de gegevens vertrouwelijk zijn hebt u twee opties. De eerste is toegang tot uw werkbelasting spreiden zodanig dat het object wordt benaderd gedurende een periode van tegenover tegelijkertijd worden geopend. U kunt ook het object tijdelijk kopiëren naar meerdere accounts voor opslag waardoor de totale IOP's per object en tussen opslagaccounts te verhogen. In de beperkte testen we gevonden dat ongeveer 25 virtuele machines tegelijkertijd een blob 100GB parallel (elke virtuele machine is het downloaden met 32 threads parallelizing) kunnen downloaden. Als u had 100 clients die toegang tot het object, eerst kopiëren naar een tweede storage-account en vervolgens de eerste 50 VM's toegang tot de eerste blob en het tweede 50 VM's toegang tot de tweede blob. Resultaten varieert afhankelijk van het gedrag van uw toepassingen zodat u dit tijdens het ontwerp moet testen. 

#### <a name="subheading16"></a>Bandbreedte en bewerkingen per Blob
U kunt lezen of schrijven naar één blob op maximaal 60 MB per seconde (dit is ongeveer 480 Mbps die groter is dan de mogelijkheden van veel client-side netwerken (met inbegrip van de fysieke NIC op het clientapparaat). Bovendien ondersteunt één blob maximaal 500 aanvragen per seconde. Als u meerdere clients hebt die moeten worden gelezen van de dezelfde blob hebt en u kunt deze limiet overschrijden, moet u overwegen een CDN voor het distribueren van de blob.  

Zie voor meer informatie over de doorvoer van het doel voor blobs [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).  

### <a name="copying-and-moving-blobs"></a>Kopiëren en verplaatsen van BLOB 's
#### <a name="subheading17"></a>Blob kopiëren
De REST-API versie 2012-02-12-opslag is de mogelijkheid nuttig voor het kopiëren van BLOB's tussen accounts geïntroduceerd: een clienttoepassing kunt opgeven dat de storage-service naar een blob kopiëren uit een andere bron (mogelijk in een ander opslagaccount) en vervolgens laat u de service de kopie asynchroon uitvoeren. Dit kan aanzienlijk verminderen de bandbreedte nodig voor de toepassing wanneer u gegevens uit andere opslagaccounts migreert omdat u niet wilt downloaden en de gegevens te uploaden.  

Een overweging is echter dat bij het kopiëren tussen opslagaccounts, er geen tijd garantie is op wanneer het exemplaar wordt voltooid. Als uw toepassing een kopie van de blob snel onder uw beheer te voltooien moet, is het wellicht beter kopiëren van de blob door te downloaden naar een virtuele machine en vervolgens uploaden naar de bestemming.  Zorg ervoor dat het exemplaar wordt uitgevoerd door een virtuele machine uitgevoerd in dezelfde Azure-regio, anders netwerkomstandigheden mogelijk (en mogelijk wordt) invloed hebben op de prestaties van uw exemplaar voor volledige voorspelbaarheid in dit geval.  Bovendien kunt u de voortgang van een asynchrone kopie programmatisch bewaken.  

Houd er rekening mee dat snel exemplaren binnen hetzelfde opslagaccount zelf in het algemeen zijn voltooid.  

Zie voor meer informatie [Blob kopiëren](http://msdn.microsoft.com/library/azure/dd894037.aspx).  

#### <a name="subheading18"></a>AzCopy gebruiken
Het Azure Storage-team heeft een opdrachtregelprogramma 'AzCopy' die is bedoeld om u te helpen bij bulksgewijs overbrengen veel blobs naar, van, en tussen opslagaccounts.  Dit hulpprogramma is geoptimaliseerd voor dit scenario en maximale overdrachtssnelheid kunt bereiken.  Het gebruik ervan wordt aangemoedigd voor bulksgewijs laden, gedownload en kopieer scenario's. Zie voor meer informatie over deze en downloaden, [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).  

#### <a name="subheading19"></a>Azure Import/Export-Service
Voor zeer grote hoeveelheden gegevens (meer dan 1TB) biedt Azure Storage de Import/Export-service, die staat voor het uploaden en downloaden van blob-opslag van back-upfunctie harde schijven.  U kunt uw gegevens op een harde schijf te plaatsen en naar Microsoft verzenden voor het uploaden of een lege harde schijf naar Microsoft verzonden om gegevens te downloaden.  Zie voor meer informatie [gebruik van de Microsoft Azure Import/Export-Service gegevens overdragen naar Blob Storage](../storage-import-export-service.md).  Dit kan zijn veel efficiënter dan uploaden/downloaden van dit volume aan gegevens via het netwerk.  

### <a name="subheading20"></a>Met behulp van metagegevens
De blob-service ondersteunt head-aanvragen, met inbegrip van metagegevens over de blob. Bijvoorbeeld, als uw toepassing de EXIF-gegevens buiten een foto vereist, kan deze ophalen van de foto en pakt. U bandbreedte besparen en de prestaties verbeteren door uw toepassing kan de EXIF gegevens opslaan in de blob-metagegevens wanneer de toepassing de foto geüpload: u kunt vervolgens ophalen EXIF-gegevens in de metagegevens met alleen een HEAD-aanvragen, opslaan van veel bandbreedte en de verwerkingstijd nodig om op te halen van de EXIF gegevens door elke keer dat de blob wordt gelezen. Dit is nuttig in scenario's waarbij u alleen de metagegevens en niet de volledige inhoud van een blob hoeft.  Houd er rekening mee dat alleen 8 KB van metagegevens per blob (de service accepteert geen een aanvraag voor het opslaan van meer dan die), kunnen worden opgeslagen zodat als de gegevens in die grootte niet past, kunt u mogelijk niet gebruik deze benadering.  

Zie voor een voorbeeld van het ophalen van metagegevens van een blob met .NET [Set eigenschappen ophalen en metagegevens en](../blobs/storage-properties-metadata.md).  

### <a name="uploading-fast"></a>Snel uploaden
Als u wilt uploaden snel blobs, de eerste vraag te beantwoorden is: weet u het uploaden van een blob of veel?  Gebruik de onderstaande richtlijnen voor het bepalen van de juiste methode te gebruiken, afhankelijk van uw scenario.  

#### <a name="subheading21"></a>Snel uploaden een groot blob
Als u wilt uploaden snel één grote blob, uploaden uw clienttoepassing de blokken of pagina's parallel (met name Houd ook rekening met de schaalbaarheidsdoelen voor afzonderlijke blobs en het opslagaccount in zijn geheel).  Houd er rekening mee dat het officiële Microsoft geleverde RTM opslag clientbibliotheken (.NET, Java) de mogelijkheid om dit te doen.  Voor elk van de bibliotheken, gebruikt u de hieronder opgegeven object of de eigenschap in te stellen van het niveau van gelijktijdigheid van taken:  

* .NET: Set ParallelOperationThreadCount op een object BlobRequestOptions moet worden gebruikt.
* Java/Android: BlobRequestOptions.setConcurrentRequestCount() gebruiken
* Node.js: Gebruik parallelOperationThreadCount op de aanvraag-opties of op de blob-service.
* C++: Gebruik de methode blob_request_options::set_parallelism_factor.

#### <a name="subheading22"></a>Veel blobs uploaden snel
Als u wilt uploaden snel veel blobs, blobs parallel te uploaden. Dit is sneller dan één BLOB's tegelijk met parallel blok uploads worden geüpload omdat het uploaden van de verspreid over meerdere partities van de storage-service. Één blob biedt alleen ondersteuning voor een doorvoer van 60 MB per seconde (ongeveer 480 Mbps). Op het moment van schrijven ondersteunt een account op basis van VS LRS maximaal 20 Gbps inkomend die veel meer dan de doorvoer wordt ondersteund door een afzonderlijke blob.  [AzCopy](#subheading18) uploads parallel standaard uitvoert, en wordt aanbevolen voor dit scenario.  

### <a name="subheading23"></a>Het juiste type blob kiezen
Azure Storage ondersteunt twee typen blobs: *pagina* blobs en *blok* blobs. Uw keuze van het blobtype heeft invloed op de prestaties en schaalbaarheid van uw oplossing voor een bepaalde gebruiksscenario. Blok-blobs geschikt zijn wanneer u wilt uploaden van grote hoeveelheden gegevens efficiënt: bijvoorbeeld wellicht een clienttoepassing foto's of video uploaden naar blob storage. Pagina-blobs geschikt als de toepassing moet voor het uitvoeren van willekeurige schrijfbewerkingen op de gegevens zijn: bijvoorbeeld Azure VHD's worden opgeslagen als pagina-blobs.  

Zie voor meer informatie [blok-Blobs, toevoeg-Blobs en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).  

## <a name="tables"></a>Tabellen
Naast de bewezen [alle Services](#allservices) hierboven beschreven, de volgende procedures bewezen name van toepassing op de tabelservice.  

### <a name="subheading24"></a>Tabel-specifieke Schaalbaarheidsdoelen
Naast de bandbreedtebeperkingen van een volledige storage-account hebben tabellen de volgende specifieke schaalbaarheidslimiet.  Houd er rekening mee dat het systeem taakverdeling van als het verkeer toeneemt, maar als het verkeer plotselinge bursts heeft, kunt u mogelijk niet onmiddellijk ophalen van dit volume van doorvoer.  Als het patroon bursts heeft, moet u verwachten Zie bandbreedtebeperking en/of time-outs tijdens de burst omdat de opslaggroep service automatisch load saldo's uit de tabel.  In het algemeen langzaam afhandelen heeft betere resultaten als dit de systeemtijd waarop taakverdeling wordt toegepast op de juiste wijze biedt.  

#### <a name="entities-per-second-account"></a>Entiteiten per seconde (Account)
De schaalbaarheidslimiet voor toegang tot tabellen voor is maximaal 20.000 entiteiten (1KB elke) per seconde voor een account.  In het algemeen elke entiteit die wordt ingevoegd, bijgewerkt, verwijderd of gescand tellingen voor dit doel.  Dus een batchinvoeging met 100 entiteiten geteld als 100 entiteiten.  Een query waarmee 1000 entiteiten wordt gescand en retourneert 5 geteld als 1000 entiteiten.  

#### <a name="entities-per-second-partition"></a>Entiteiten per seconde (partitie)
Binnen een enkele partitie is het doel schaalbaarheid voor toegang tot tabellen 2.000 entiteiten (1KB elke) per seconde, met behulp van de dezelfde tellen zoals beschreven in de vorige sectie.  

### <a name="configuration"></a>Configuratie
Deze sectie vindt u enkele snelle configuratie-instellingen die u gebruiken kunt om belangrijke prestatieverbeteringen in de tabelservice:  

#### <a name="subheading25"></a>Gebruik JSON
Vanaf versie van de service storage 2013-08-15, ondersteunt de tabelservice het gebruik van JSON in plaats van de AtomPub op basis van een XML-indeling voor het overbrengen van gegevens in een tabel. Dit bespaart nettolading grootten met zoveel 75% en kan de prestaties van uw toepassing aanzienlijk verbeterd.

Zie voor meer informatie het bericht [Microsoft Azure-tabellen: Introducing JSON](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/05/windows-azure-tables-introducing-json.aspx) en [indeling van nettolading voor tabel servicebewerkingen](http://msdn.microsoft.com/library/azure/dn535600.aspx).

#### <a name="subheading26"></a>Nagle uitschakelen
Nagle van algoritme is algemeen geïmplementeerd via TCP/IP-netwerken als ter verbetering van netwerkprestaties. Het is echter niet optimaal in alle gevallen (zoals interactieve omgevingen). Voor Azure Storage van Nagle-algoritme heeft een nadelige invloed op de prestaties van aanvragen voor de tabel en queue-services en moet u dit indien mogelijk uitschakelen.  

Zie voor meer informatie onze blogbericht [van Nagle-algoritme is niet beschrijvende kleine aanvragen](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx), waarin wordt uitgelegd waarom Nagle van algoritme slecht communiceert met table en queue aanvragen en laat zien hoe deze in uw clienttoepassing uitschakelen.  

### <a name="schema"></a>Schema
Hoe u vertegenwoordigen en uw gegevens een query is de grootste één factor die invloed heeft op de prestaties van de tabelservice. Terwijl elke toepassing verschilt, is deze sectie geeft een overzicht enkele algemene beproefde procedures die betrekking hebben op:  

* Tabelontwerp
* Efficiënt query 's
* Efficiënte Gegevensupdates  

#### <a name="subheading27"></a>Tabellen en partities
Tabellen worden onderverdeeld in partities. Elke entiteit die is opgeslagen in een partitie dezelfde partitiesleutel gedeeld en heeft een unieke rij-sleutel bij het identificeren ervan in die partitie. Partities voordelen bieden, maar ook limieten voor schaalbaarheid veroorzaken.  

* Voordelen: U kunt entiteiten in dezelfde partitie in een enkel, atomic, batchtransactie met maximaal 100 afzonderlijke opslagbewerkingen (maximaal 4MB totale grootte) bijwerken. Ervan uitgaande dat hetzelfde aantal entiteiten worden opgehaald, u kunt ook een query gegevens binnen één partitie efficiënter dan gegevens meerdere partities (Hoewel gelezen op voor verdere aanbevelingen voor het opvragen van tabelgegevens).
* Schaalbaarheidslimiet: toegang tot de entiteiten die zijn opgeslagen in één partitie kan niet worden taakverdeling omdat partities atomic batchtransacties ondersteunen. Daarom is het doel van de schaalbaarheid van de partitie van een afzonderlijke tabel lager is dan voor de tabelservice als geheel.  

Vanwege deze kenmerken van tabellen en partities, moet u overstapt op de volgende principes:  

* Gegevens die uw clienttoepassing regelmatig bijgewerkt of een query uitgevoerd in dezelfde logische eenheid van werk moeten zich bevinden in dezelfde partitie.  Dit kan zijn omdat schrijfbewerkingen is aggregeren van uw toepassing, of omdat u wilt profiteren van atomaire batchbewerkingen.  Bovendien kan gegevens in één partitie worden efficiënter doorzocht in één query dan gegevens meerdere partities.
* Gegevens die uw clienttoepassing komt niet invoegen/bijwerken of query op in dezelfde logische eenheid van werk (één query of batch-update) moet zich bevinden op afzonderlijke partities.  Een belangrijke opmerking is dat er geen limiet voor het aantal partitiesleutels in één tabel, dus met miljoenen partitiesleutels geen probleem is en heeft geen invloed op prestaties.  Bijvoorbeeld, als uw toepassing een populaire website met gebruikersaanmelding, is met behulp van de gebruikers-Id als de partitiesleutel een goede keuze.  

#### <a name="hot-partitions"></a>Hot partities
Een hot partitie is een waarin een onevenredige percentage van het verkeer naar een account is ontvangen en kan niet worden met gelijke taakverdeling omdat deze één partitie.  Hot partities gemaakt in het algemeen zijn twee manieren:  

##### <a name="subheading28"></a>Alleen toevoegen en voeg alleen patronen
Het patroon 'Alleen toevoegen' is een waar alle (of bijna alle) van het verkeer naar een bepaalde PK verhoogt en verkleint u volgens de huidige tijd.  Een voorbeeld is als uw toepassing de huidige datum als een partitiesleutel voor logboekgegevens gebruikt.  Dit leidt ertoe dat alle van de invoegt gaan naar de laatste partitie in de tabel en het systeem kan saldo niet laden omdat alle van schrijfbewerkingen naar het einde van de tabel gaat.  Als het volume van verkeer voor deze partitie is groter dan het doel partitie niveau schaalbaarheid en vervolgens het tot beperking leiden zal.  Is het beter om ervoor te zorgen dat verkeer wordt verzonden naar meerdere partities bevat, waarmee taakverdeling de aanvragen in de tabel.  

##### <a name="subheading29"></a>Intensief verkeer gegevens
Als uw partitieschema in een enkele partitie met alleen gegevens die veel meer dan andere partities die worden gebruikt resulteert, ziet u mogelijk ook beperking als dat partitie het doel van de schaalbaarheid van één partitie nadert.  Is het beter om ervoor te zorgen dat uw partitieschema resulteert in geen enkele partitie de schaalbaarheidsdoelen nadert.  

#### <a name="querying"></a>Uitvoeren van query 's
Deze sectie beschrijft bewezen voor query's in de tabelservice.  

##### <a name="subheading30"></a>Querybereik
Er zijn verschillende manieren om op te geven van het bereik van entiteiten aan query.  Hier volgt een bespreking van elk worden gebruikt.  

In het algemeen te voorkomen dat scans (query's groter is dan één entiteit), maar als u scannen moet, probeert de gegevens te ordenen zodat uw scans gegevens zonder scannen of het opvragen van grote hoeveelheden entiteiten hoeft u niet ophalen.  

###### <a name="point-queries"></a>Punt-query 's
Een punt query haalt precies één entiteit. Dit gebeurt door het opgeven van de partitiesleutel en de rijsleutel van de entiteit ophalen. Deze query's zijn zeer efficiënte en u zoveel mogelijk deze moet gebruiken.  

###### <a name="partition-queries"></a>Partitie-query 's
Een partitiequery is een query waarmee een set gegevens die een algemene partitiesleutel deelt opgehaald. De query is doorgaans een bereik van de sleutelwaarden rij of een bereik van waarden voor een bepaalde eigenschap van entiteit naast een partitiesleutel. Deze zijn minder efficiënt dan punt query's en moeten spaarzaam worden gebruikt.  

###### <a name="table-queries"></a>Tabel-query 's
Een tabelquery is een query waarmee een verzameling entiteiten die een algemene partitiesleutel niet deelt worden opgehaald. Deze query's zijn niet efficiënt en u indien mogelijk moet voorkomen.  

##### <a name="subheading31"></a>Query-dichtheid
Een andere belangrijke factor in de efficiëntie van de query is het aantal entiteiten die zijn geretourneerd in vergelijking met het aantal entiteiten gescand om te zoeken naar de resulterende set. Als uw toepassing wordt een tabelquery met een filter voor een eigenschapswaarde slechts 1% van de gegevens shares, de query wordt scannen 100 entiteiten voor elke één entiteit wordt uitgevoerd. De tabel schaalbaarheidsdoelen besproken eerder alle betrekking op het aantal entiteiten gescand en niet het aantal entiteiten dat is geretourneerd: een lage query dichtheid eenvoudig ertoe leiden dat de tabelservice om te beperken van uw toepassing omdat zoveel entiteiten voor het ophalen van de entiteit die u zoekt moet worden gecontroleerd.  Zie de sectie hieronder op [denormalization](#subheading34) voor meer informatie over hoe u dit probleem te voorkomen.  

##### <a name="limiting-the-amount-of-data-returned"></a>Beperken van de hoeveelheid gegevens die worden geretourneerd
###### <a name="subheading32"></a>Filteren
Wanneer u een query wordt retourneert entiteiten die u hoeft niet in de clienttoepassing weet, kunt u overwegen een filter op Reduceer de grootte van de resulterende set. Terwijl de entiteiten die niet zijn geretourneerd naar de client wordt nog steeds voor de limieten voor schaalbaarheid meetellen, verbeteren de toepassingsprestaties van uw vanwege de grootte van de nettolading verminderde netwerk en de verminderde aantal entiteiten dat is verwerkt door de clienttoepassing.  Zie boven notitie op [Query dichtheid](#subheading31), echter – de schaalbaarheidsdoelen hebben betrekking op het aantal entiteiten gescand, zodat een query die veel entiteiten gefilterd mogelijk nog steeds in de beperking, zelfs als er enkele entiteiten worden geretourneerd.  

###### <a name="subheading33"></a>Projectie
Als u de clienttoepassing een beperkt aantal eigenschappen van de entiteiten in de tabel moet, kunt u projectie beperkt de omvang van de geretourneerde gegevensset. Net als bij filteren, helpt dit minder netwerkbelasting en client verwerken.  

##### <a name="subheading34"></a>Denormalization
In tegenstelling tot het werken met relationele databases, wordt de beproefde procedures voor het opvragen van tabelgegevens efficiënt leiden tot denormalizing van uw gegevens. Dat wil zeggen, het dupliceren van dezelfde gegevens in meerdere entiteiten (één voor elke sleutel die u kunt de gegevens zoeken) Beperk het aantal entiteiten waarmee een query moet worden gescand om te zoeken de gegevens van de client moeten in plaats uw toepassing moet hebben om te scannen van grote aantallen entiteiten vinden van de gegevens.  Bijvoorbeeld, een webwinkel u kunt in een volgorde beide door de klant-ID vinden (Geef me van de klant orders) en de datum (Geef me orders op een datum).  In de Table Storage, is het aanbevolen voor het opslaan van de entiteit (of een verwijzing naar het) tweemaal – één keer met de naam van de tabel PK en RK te vergemakkelijken zoeken door de klant-ID, eenmaal vinden door de datum mogelijk te maken.  

#### <a name="insertupdatedelete"></a>Invoegen, bijwerken, verwijderen
Deze sectie beschrijft bewezen voor het wijzigen van de entiteiten die zijn opgeslagen in de tabelservice.  

##### <a name="subheading35"></a>Batchverwerking
Batchtransacties bekend als entiteit groep transacties (ETG) in Azure Storage; alle bewerkingen binnen een ETG moeten zich op één partitie in één tabel. Waar mogelijk, ETGs voor invoegen, bijwerken en verwijderen in batches gebruiken. Dit vermindert het aantal retouren van uw clienttoepassing op de server, vermindert het aantal factureerbare transacties (een ETG telt als een enkele transactie voor facturering en mag maximaal 100 opslagbewerkingen) en maakt atomic updates (alle bewerkingen slagen of allemaal binnen een ETG mislukken). Omgevingen met hoge latentie, zoals mobiele apparaten profiteren aanzienlijk ETGs te gebruiken.  

##### <a name="subheading36"></a>Upsert
Gebruik tabel **Upsert** operations waar mogelijk. Er zijn twee soorten **Upsert**, die beide kunnen zijn efficiënter dan een traditionele **invoegen** en **Update** bewerkingen:  

* **InsertOrMerge**: Gebruik deze optie als u wilt uploaden een subset van de eigenschappen van de entiteit, maar niet zeker weet of de entiteit bestaat al. Als de entiteit bestaat, deze aanroep bijgewerkt met de eigenschappen die zijn opgenomen in de **Upsert** bewerking, en blijven alle bestaande eigenschappen zoals ze zijn, als de entiteit niet bestaat, wordt de nieuwe entiteit ingevoegd. Dit is vergelijkbaar met het gebruik van projectie in een query in dat u alleen hoeft voor het uploaden van de eigenschappen die u wilt wijzigen.
* **InsertOrReplace**: Gebruik deze optie als u wilt uploaden van een geheel nieuwe entiteit, maar u niet zeker weet of deze al bestaat. U moet dit alleen gebruiken als u weet dat het recent geüploade entiteit volledig juist is, omdat de oude entiteit volledig overschrijft. Bijvoorbeeld, wilt u bijwerken van de entiteit die de huidige locatie van een gebruiker slaat ongeacht de toepassing al dan niet eerder locatiegegevens voor de gebruiker opgeslagen heeft. de nieuwe locatie entiteit is voltooid en u hoeft geen gegevens uit een eerdere entiteit.

##### <a name="subheading37"></a>De gegevensreeks opslaan in een enkele entiteit
Een toepassing slaat soms, een reeks met gegevens die vaak nodig is om op te halen in één keer: bijvoorbeeld een toepassing kan CPU-gebruik gedurende een periode bijhouden om te kunnen tekenen van een rolling grafiek van de gegevens van de afgelopen 24 uur. Een aanpak is om een Tabelentiteit per uur, met elke entiteit die een specifiek uur en het CPU-gebruik op te slaan voor dat uur. De toepassing moet u deze gegevens getekend, voor het ophalen van de entiteiten die de gegevens van de 24 uur van de meest recente.  

U kunt ook het CPU-gebruik voor elk uur als een afzonderlijke eigenschap van een enkele entiteit kan opslaan van uw toepassing: voor het bijwerken van elk uur toepassingen kunnen gebruikmaken van één **InsertOrMerge Upsert** aanroep van de waarde voor de meest recente uur bijgewerkt. Als u wilt de gegevens wordt getekend, de toepassing alleen hoeft op te halen één entiteit in plaats van 24, maken voor een zeer efficiënte query (Zie hierboven discussie op [bereik query](#subheading30)).

##### <a name="subheading38"></a>Opslaan van gestructureerde gegevens in BLOB 's
Soms gestructureerde gegevens werkt zoals moet worden gegaan in tabellen, maar bereiken van entiteiten worden altijd samen opgehaald en kan worden batch ingevoegd.  Een goed voorbeeld hiervan is een logboekbestand.  In dit geval kunt u enkele minuten aan logboeken batch, voegt u deze, en vervolgens u enkele minuten aan logboeken op een tijdstip ook altijd een ophaalt.  In dit geval voor prestaties verdient het aanbeveling blobs in plaats van tabellen, gebruikt, omdat u het aantal objecten geschreven/geretourneerd, evenals meestal het aantal aanvragen die moeten worden aangebracht aanzienlijk kan verminderen.  

## <a name="queues"></a>Wachtrijen
Naast de bewezen [alle Services](#allservices) hierboven beschreven, de volgende procedures bewezen name van toepassing op de queue-service.  

### <a name="subheading39"></a>Limieten voor schaalbaarheid
Een enkele wachtrij kunt ongeveer 2.000-berichten verwerken (1KB elke) per seconde (elke AddMessage GetMessage en DeleteMessage aantal als hier een bericht). Als dit onvoldoende voor uw toepassing is, moet u meerdere wachtrijen gebruiken en de berichten verdeeld over deze.  

Huidige schaalbaarheidsdoelen op weergeven [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).  

### <a name="subheading40"></a>Nagle uitschakelen
Zie de sectie op de configuratie van de tabel die het algoritme Nagle besproken: het algoritme Nagle is doorgaans niet voor de prestaties van de wachtrij aanvragen en moet u dit uitschakelen.  

### <a name="subheading41"></a>Grootte van het bericht
Wachtrij afname prestaties en schaalbaarheid als bericht grootte toeneemt. U moet alleen de informatie die de ontvanger moet plaatsen in een bericht.  

### <a name="subheading42"></a>Batch ophalen
U kunt maximaal 32 berichten ophalen uit een wachtrij in één bewerking. Verminder het aantal interactie van de clienttoepassing, dit vooral geschikt voor omgevingen zoals mobiele apparaten is, kan dit met een hoge latentie.  

### <a name="subheading43"></a>Wachtrij Polling-Interval
De meeste toepassingen pollen berichten van een wachtrij, wat kan een van de grootste bronnen van transacties voor die toepassing. Selecteer uw polling-interval verstandig: polling te vaak kan ervoor zorgen dat uw toepassing om de schaalbaarheidsdoelen voor de wachtrij. 200.000 transacties voor $0,01 (op het moment van schrijven) is één processor polling nadat elke seconde gedurende een maand zou u minder dan 15 cent dus kosten echter niet doorgaans een factor die invloed heeft op uw keuze van polling-interval.  

Zie voor kosteninformatie up-to-date [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

### <a name="subheading44"></a>UpdateMessage
U kunt **UpdateMessage** Verhoog de time-out voor onzichtbaarheid of bij te werken informatie over de status van een bericht. Dit is een krachtige, houd er rekening mee dat elke **UpdateMessage** bewerking telt naar het doel van de schaalbaarheid. Dit kan echter zijn dat een benadering veel efficiënter dan wanneer een werkstroom die een taak van de ene wachtrij aan de volgende doorgegeven elke stap van de taak is voltooid. Met behulp van de **UpdateMessage** bewerking kan de toepassing voor het opslaan van de taakstatus op het bericht en vervolgens doorgaan met werken in plaats van het bericht voor de volgende stap van de taak opnieuw queuing telkens wanneer een stap is voltooid.  

Zie voor meer informatie het artikel [hoe: de inhoud van een bericht in de wachtrij wijzigen](../queues/storage-dotnet-how-to-use-queues.md#change-the-contents-of-a-queued-message).  

### <a name="subheading45"></a>Toepassingsarchitectuur
Wachtrijen moet u uw toepassingsarchitectuur schaalbare maken. Hieronder vindt u enkele manieren kunt u wachtrijen ervoor dat uw toepassing meer schaalbare gebruiken:  

* U kunt wachtrijen gebruiken voor het maken van achterstanden voor verwerking en vloeiend werkbelastingen in uw toepassing. Bijvoorbeeld, u kan in de wachtrij aanvragen van gebruikers om uit te voeren processor intensief werken zoals geüploade afbeeldingen vergroten of verkleinen.
* U kunt wachtrijen gebruiken loskoppelen van de onderdelen van uw toepassing zodat u afzonderlijk kunt schalen. Een webfront-end kan bijvoorbeeld enquêteresultaten van gebruikers plaatsen in een wachtrij voor latere analyse en opslag. U kunt meer exemplaren van worker-rol voor het verwerken van de wachtrijgegevens zo nodig kan toevoegen.  

## <a name="conclusion"></a>Conclusie
In dit artikel besproken enkele van de meest voorkomende bewezen procedures voor het optimaliseren van de prestaties bij gebruik van Azure Storage. Wij raden elke toepassingsontwikkelaar aan om toepassingen te beoordelen aan de hand van elk van de bovenstaande procedures en de aanbevelingen te overwegen voor het verkrijgen van uitstekende prestaties voor toepassingen die gebruikmaken van Azure Storage.