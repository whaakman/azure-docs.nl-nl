---
title: Ontwerphandleiding voor Azure Storage-tabel | Microsoft Docs
description: Ontwerp schaalbare en zodat tabellen in de Azure-tabelopslag
services: cosmos-db
documentationcenter: na
author: mimig1
manager: tadb
editor: tysonn
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: a5511b8b2e76c6c651a8e05bda1322293601c92c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-storage-table-design-guide-designing-scalable-and-performant-tables"></a>Ontwerphandleiding voor Azure Storage-tabel: Het ontwerpen van schaalbare en de zodat tabellen
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Ontwerp schaalbare en zodat tabellen, moet u rekening houden met een aantal factoren, zoals prestaties, schaalbaarheid en kosten. Als u eerder schema's voor relationele databases hebt ontworpen, deze overwegingen u bekend zijn, maar er zijn een aantal overeenkomsten tussen de Azure Table storage servicemodel en relationele modellen, maar er zijn ook veel belangrijke verschillen. Deze verschillen worden doorgaans leiden tot zeer verschillende ontwerpen die lijkt erg intuïtief of onjuiste naar iemand bekend bent met relationele databases, maar maak die zinvol als u voor een NoSQL-sleutel/waarde-archief zoals de Azure Table-service ontwerpt. Veel van de verschillen in het ontwerp worden doorgevoerd in het feit dat de tabel-service is ontworpen ter ondersteuning van cloud-toepassingen met miljarden entiteiten (rijen in een relationele database-terminologie) van gegevens of voor gegevenssets die zeer hoge transactie volumes moet ondersteunen: daarom moet u anders nadenken over hoe u uw gegevens opslaat en u begrijpt hoe de tabelservice werkt. Uw oplossing voor het schalen van veel meer (en, tegen lagere kosten) kunt inschakelen door een goed ontworpen NoSQL-gegevensarchief dan een oplossing die gebruikmaakt van een relationele database. Deze handleiding helpt u met deze onderwerpen.  

## <a name="about-the-azure-table-service"></a>Over de Azure Table-service
Deze sectie worden enkele van de belangrijkste functies van de tabel-service die vooral relevant voor ontwerpen voor prestaties en schaalbaarheid zijn. Als u niet bekend met Azure Storage en de tabelservice bent, eerst lezen [Inleiding tot Microsoft Azure Storage](../storage/common/storage-introduction.md) en [aan de slag met Azure Table Storage met .NET](table-storage-how-to-use-dotnet.md) voordat de rest van dit artikel wordt gelezen. Hoewel deze handleiding voor de tabel-service is, wordt het betekenen dat sommige bespreking van de Azure-wachtrij en de Blob-services, en hoe u ze samen met de tabel-service in een oplossing mogelijk gebruiken.  

Wat is de tabel-service? Als u van de naam verwacht, wordt in de tabel-service tabelvorm gebruikt voor het opslaan van gegevens. Elke rij van de tabel een entiteit vertegenwoordigt in de standaard terminologie en de kolommen opslaan van de verschillende eigenschappen van die entiteit. Elke entiteit heeft een paar sleutels om uniek te identificeren en een timestamp-kolom die de tabel-service gebruikt om bij te houden als de entiteit voor het laatst is bijgewerkt (dit gebeurt automatisch en u kunt handmatig de tijdstempel niet overschrijven met een willekeurige waarde). De tabel-service gebruikt deze tijdstempel laatst is gewijzigd (LMT) voor het beheren van optimistische gelijktijdigheid.  

> [!NOTE]
> De tabel service REST-API-bewerkingen ook retourneren een **ETag** waarde die deze is afgeleid van de laatste wijziging tijdstempel (LMT). In dit document gebruiken we de termen ETag en LMT door elkaar omdat ze naar de onderliggende gegevens verwijzen.  
> 
> 

Het volgende voorbeeld ziet een eenvoudige tabelontwerp voor het opslaan van entiteiten werknemer en afdeling. Veel van de voorbeelden die verderop in deze handleiding zijn gebaseerd op dit eenvoudige ontwerp.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
<th></th>
</tr>
<tr>
<td>Marketing</td>
<td>00001</td>
<td>2014-08-22T00:50:32Z</td>
<td>
<table>
<tr>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td>Jan</td>
<td>Hall</td>
<td>34</td>
<td>donh@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>00002</td>
<td>2014-08-22T00:50:34Z</td>
<td>
<table>
<tr>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td>jun</td>
<td>CaO</td>
<td>47</td>
<td>junc@contoso.com</td>
</tr>
</table>
</tr>
<tr>
<td>Marketing</td>
<td>Afdeling</td>
<td>2014-08-22T00:50:30Z</td>
<td>
<table>
<tr>
<th>Naam van de afdeling</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Marketing</td>
<td>153</td>
</tr>
</table>
</td>
</tr>
<tr>
<td>Verkoop</td>
<td>00010</td>
<td>2014-08-22T00:50:44Z</td>
<td>
<table>
<tr>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td>Ken</td>
<td>Kwok</td>
<td>23</td>
<td>kenk@contoso.com</td>
</tr>
</table>
</td>
</tr>
</table>


Tot nu toe lijkt dit erg op een tabel in een relationele database met de belangrijkste verschillen, wordt de verplichte kolommen en de mogelijkheid voor het opslaan van meerdere Entiteitstypen in dezelfde tabel. Bovendien elk van de gebruiker gedefinieerde eigenschappen zoals **FirstName** of **leeftijd** heeft een gegevenstype, zoals geheel getal of tekenreeks, net zoals een kolom in een relationele database. Hoewel in tegenstelling tot in een relationele database, de schema-minder aard van de tabel-service betekent dat een eigenschap moet niet hetzelfde gegevenstype voor elke entiteit. Complexe gegevenstypen opslaan in één eigenschap, moet u een zoals JSON of XML-serialisatie-indeling. Zie voor meer informatie over de tabel-service, zoals ondersteunde gegevenstypen, ondersteunde datumbereiken, naamgevingsregels en beperkingen voor [inzicht in de tabel Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Zoals u ziet, uw keuze van **PartitionKey** en **RowKey** is van cruciaal belang goede tabelontwerp. Elke entiteit die is opgeslagen in een tabel moet een unieke combinatie van **PartitionKey** en **RowKey**. Als u met de sleutels in een relationele database, de **PartitionKey** en **RowKey** waarden worden geïndexeerd voor het maken van een geclusterde index waarmee u snel te zoeken; echter, de tabel-service maakt geen secundaire indexen zodat deze de slechts twee geïndexeerde eigenschappen (enkele van de patronen die verderop tonen hoe u deze duidelijk beperking kunt omzeilen).  

Een tabel bestaat uit een of meer partities en zoals u ziet, worden veel van de ontwerpbeslissingen die u aanbrengt rond het kiezen van een geschikte **PartitionKey** en **RowKey** optimaliseren van uw oplossing. Een oplossing kan bestaan uit slechts één tabel die de entiteiten die zijn onderverdeeld in partities bevat, maar doorgaans een oplossing heeft meerdere tabellen. Tabellen kunt u logisch ordenen van uw entiteiten, helpen bij het beheren van toegang tot de gegevens met behulp van toegangsbeheerlijsten en u kunt een hele tabel met een één opslag-bewerking neerzetten.  

### <a name="table-partitions"></a>Tabelpartities
De accountnaam, de tabelnaam en **PartitionKey** samen bepalen de partitie in de storage-service waar de entiteit in de tabelservice worden opgeslagen. Onderdeel van het adresseringsschema voor entiteiten zijn, partities een bereik voor transacties definiëren (Zie [entiteit groepstransacties](#entity-group-transactions) hieronder), en de basis vormen van de manier waarop de tabelservice schaalt. Zie voor meer informatie over partities [Azure Storage Scalability and Performance Targets](../storage/common/storage-scalability-targets.md).  

Een afzonderlijke knooppunten services in de tabel-service, een of meer partities en de service kan worden geschaald voltooien door dynamisch taakverdeling partities over knooppunten. Als een knooppunt belast wordt, de tabelservice kunt *splitsen* het bereik van de partities onderhouden door dat knooppunt op verschillende knooppunten; wanneer verkeer subsidies, de service kan *samenvoegen* de partitie kan variëren van stille knooppunten back naar één knooppunt.  

Zie het artikel voor meer informatie over de interne details van de tabel-service en met name hoe de service partities beheert, [Microsoft Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).  

### <a name="entity-group-transactions"></a>Entiteit groep transacties
Entiteit groepstransacties (EGTs) zijn in de tabel-service de enige ingebouwde mechanisme voor het uitvoeren van atomaire updates tussen meerdere entiteiten. EGTs worden ook aangeduid als *batch transacties* in sommige documentatie. EGTs werkt alleen voor entiteiten die zijn opgeslagen in dezelfde partitie (share dezelfde partitiesleutel in een bepaalde tabel), dus telkens wanneer u atomic transactionele gedrag tussen meerdere entiteiten moet, u ervoor zorgen moet dat deze entiteiten in dezelfde partitie zijn. Dit is vaak een reden voor meerdere tabellen niet gebruiken voor andere entiteitstypen en meerdere Entiteitstypen houden in dezelfde tabel (en de partitie). Een enkele EGT kan werken op maximaal 100 entiteiten.  Als u meerdere gelijktijdige EGTs voor de verwerking is het belangrijk om ervoor te zorgen dat die EGTs worden niet uitgevoerd op de entiteiten die voor EGTs gelden anders verwerken vertraging verzenden.

EGTs ook leiden tot een mogelijke waarde om te beoordelen in uw ontwerp: de schaalbaarheid van uw toepassing met behulp van meer partities wordt verhoogd omdat Azure heeft meer mogelijkheden voor aanvragen voor taakverdeling over de knooppunten, maar dit kan de mogelijkheid van uw toepassing uit te voeren atomische transacties sterke consistentie voor uw gegevens beperken. Bovendien, er zijn specifieke schaalbaarheidsdoelen op het niveau van een partitie die de doorvoer van transacties die u voor één knooppunt verwachten kunt kan beperken: Zie voor meer informatie over de schaalbaarheidsdoelen voor Azure storage-accounts en de tabelservice [Azure Storage Scalability and Performance Targets](../storage/common/storage-scalability-targets.md). Latere secties van deze handleiding worden verschillende ontwerp strategieën die u helpen beheren zoals deze verschillen en bespreken het beste de partitiesleutel op basis van de specifieke vereisten van uw clienttoepassing kiezen.  

### <a name="capacity-considerations"></a>Overwegingen voor capaciteitsplanning
De volgende tabel bevat enkele van de belangrijkste waarden moet denken bij het ontwerpen van een oplossing voor tabel-service:  

| Totale capaciteit van een Azure storage-account | 500 TB |
| --- | --- |
| Aantal tabellen in een Azure storage-account |Alleen beperkt door de capaciteit van het storage-account |
| Aantal partities in een tabel |Alleen beperkt door de capaciteit van het storage-account |
| Aantal entiteiten in een partitie |Alleen beperkt door de capaciteit van het storage-account |
| Grootte van een afzonderlijke entiteit |Maximaal 1 MB van maximaal 255 eigenschappen (inclusief de **PartitionKey**, **RowKey**, en **tijdstempel**) |
| Grootte van de **PartitionKey** |Een tekenreeks maximaal 1 KB groot |
| Grootte van de **RowKey** |Een tekenreeks maximaal 1 KB groot |
| Grootte van een entiteit groep-transactie |Een transactie kan maximaal 100 entiteiten bevatten en de lading moet minder dan 4 MB groot. Een EGT kan slechts eenmaal bijwerken een entiteit. |

Zie voor meer informatie [inzicht in de tabel Service Data Model](http://msdn.microsoft.com/library/azure/dd179338.aspx).  

### <a name="cost-considerations"></a>Kosten overwegingen
Tabelopslag relatief goedkope is, maar moet u kosten maakt een schatting voor capaciteitsgebruik en het aantal transacties opnemen als onderdeel van de evaluatie van een oplossing die gebruikmaakt van de tabel-service. In veel scenario's voor het opslaan van gedenormaliseerd of dubbele gegevens om te verbeteren is de prestaties of de schaalbaarheid van uw oplossing echter een geldige benadering te laten worden. Zie voor meer informatie over prijzen [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/).  

## <a name="guidelines-for-table-design"></a>Richtlijnen voor het tabelontwerp van de
Deze lijsten geven een overzicht van enkele van de belangrijkste richtlijnen die u rekening houden moet bij het ontwerpen van uw tabellen en deze handleiding los deze allemaal in meer detail later in. Deze richtlijnen zijn heel verschillend van de richtlijnen die u doorgaans als voor het ontwerp van relationele database volgt.  

Ontwerpen van uw oplossing tabel-service worden *lezen* efficiënt:

* ***Ontwerpen voor het uitvoeren van toepassingen waarin lezen veel query's.*** Tijdens het ontwerpen van uw tabellen nadenken over de query's (met name de latentie gevoelig die) die zal worden uitgevoerd voordat u nadenken over hoe u uw entiteiten wordt bijgewerkt. Dit leidt meestal tot een efficiënte en zodat oplossing.  
* ***Geef zowel PartitionKey en RowKey in uw query's.*** *Query's wijst* zoals dit zijn de meest efficiënt query's een tabel-service.  
* ***Houd rekening met dubbele exemplaren van entiteiten opslaan.*** Tabelopslag goedkope is dus overwegen bij het opslaan van dezelfde entiteit meerdere keren (met verschillende sleutels) om in te schakelen efficiënter query's.  
* ***U kunt uw gegevens denormalizing.*** Tabelopslag goedkope is dus Overweeg denormalizing van uw gegevens. Samenvatting entiteiten bijvoorbeeld opslaan zodat query's voor statistische gegevens alleen hoeft toegang tot één entiteit.  
* ***Gebruik de samengestelde sleutel komt.*** De enige sleutels die u hebt zijn **PartitionKey** en **RowKey**. Samengestelde sleutel komt bijvoorbeeld gebruiken om in te schakelen alternatieve sleutelhash Toegangspaden naar entiteiten.  
* ***Query-projectie gebruiken.*** U kunt verminderen de hoeveelheid gegevens die u via het netwerk overdragen met behulp van query's die alleen de velden die u moet selecteren.  

Ontwerpen van uw oplossing tabel-service worden *schrijven* efficiënt:  

* ***Maak geen hot partities.*** De optie sleutels waarmee u uw aanvragen verdeeld over meerdere partities op elk moment.  
* ***Vermijd pieken in het verkeer.*** Het verkeer via een redelijke periode vloeiend en pieken in het verkeer worden vermeden.
* ***Niet per se een afzonderlijke tabel voor elk type entiteit maken.*** Wanneer u atomische transacties op Entiteitstypen vereisen, slaat u deze meerdere Entiteitstypen in dezelfde partitie in dezelfde tabel.
* ***U kunt de maximale doorvoer die moeten worden gerealiseerd.*** U moet rekening houden met de schaalbaarheidsdoelen voor de tabel-service en ervoor te zorgen dat uw ontwerp leidt niet tot u ze overschrijden.  

Als u deze handleiding leest, ziet u voorbeelden die al deze principes in de praktijk geplaatst.  

## <a name="design-for-querying"></a>Ontwerp voor het uitvoeren van query 's
Oplossingen voor tabel-service kunnen worden gelezen intensief schrijven intensief of een combinatie van beide. Deze sectie richt zich op de zaken op moet letten tijdens het ontwerpen van de service voor ondersteuning voor leesbewerkingen efficiënt uw tabel. Een ontwerp dat ondersteunt lees-en schrijfopdrachten efficiënt is meestal ook efficiënt voor schrijfbewerkingen. Er zijn echter aanvullende overwegingen op moet letten bij het ontwerpen van ondersteuning voor schrijfbewerkingen, die wordt besproken in de volgende sectie [ontwerp voor wijziging van gegevens](#design-for-data-modification).

Een goed uitgangspunt voor het ontwerpen van uw tabel-service-oplossing kunt u gegevens efficiënt te lezen is vragen "welke query mijn toepassing moeten worden uitgevoerd voor het ophalen van de benodigde gegevens uit de tabelservice?"  

> [!NOTE]
> Met de tabel-service is het belangrijk dat u het ontwerp van de juiste vooraf omdat het is moeilijk en kostbaar later wijzigen. Bijvoorbeeld in een relationele database is het vaak mogelijk prestaties om problemen te verhelpen door indexen toe te voegen aan een bestaande database: dit kan niet worden gebruikt met de tabel-service.  
> 
> 

Deze sectie legt de nadruk op de belangrijkste die u houden moet bij het ontwerpen van uw tabellen voor het uitvoeren van query's. De onderwerpen in deze sectie beschreven, zijn onder andere:

* [Hoe uw keuze van PartitionKey en RowKey van invloed is op prestaties van query 's](#how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance)
* [Een juiste PartitionKey kiezen](#choosing-an-appropriate-partitionkey)
* [Optimaliseren van query's voor de tabel-service](#optimizing-queries-for-the-table-service)
* [Sorteren van gegevens in de tabel-service](#sorting-data-in-the-table-service)

### <a name="how-your-choice-of-partitionkey-and-rowkey-impacts-query-performance"></a>Hoe uw keuze van PartitionKey en RowKey van invloed is op prestaties van query 's
De volgende voorbeelden wordt ervan uitgegaan dat de tabelservice werknemer entiteiten met de volgende structuur wordt opgeslagen (de meeste van de voorbeelden weglaten de **tijdstempel** eigenschap voor de duidelijkheid):  

| *Kolomnaam* | *Gegevenstype* |
| --- | --- |
| **PartitionKey** (naam van de afdeling) |Tekenreeks |
| **RowKey** (werknemer-Id) |Tekenreeks |
| **Voornaam** |Tekenreeks |
| **Achternaam** |Tekenreeks |
| **Leeftijd** |Geheel getal |
| **EmailAddress** |Tekenreeks |

De vorige sectie [overzicht van de service Azure Table](#overview) beschrijft een aantal van de belangrijkste functies van de service Azure Table waarvoor een directe invloed op ontwerpen voor query. Deze leiden tot de volgende algemene richtlijnen voor het ontwerpen van query's tabel-service. De filtersyntaxis gebruikt in de volgende voorbeelden wordt uit de tabelservice REST API, Zie voor meer informatie [Query entiteiten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

* Een ***punt Query*** is het meest efficiënt lookup te gebruiken en wordt aanbevolen voor grote zoekopdrachten of zoekacties laagste latentie vereisen moet worden gebruikt. Dergelijke query kunt de indexen gebruiken een afzonderlijke entiteit zeer efficiënt vinden door op te geven van zowel de **PartitionKey** en **RowKey** waarden. Bijvoorbeeld: $filter = (PartitionKey eq 'Sales') en (RowKey eq '2')  
* Tweede in de rij is een ***Bereikquery*** die gebruikmaakt van de **PartitionKey** en filters op een reeks **RowKey** waarden te retourneren van meer dan één entiteit. De **PartitionKey** waarde identificeert een specifieke partitie en de **RowKey** waarden een subset van de entiteiten in de betreffende partitie identificeren. Bijvoorbeeld: $filter = PartitionKey eq 'Verkoop en de RowKey ge' en RowKey lt t '  
* Derde beste is een ***partitie scannen*** die gebruikmaakt van de **PartitionKey** en filters op een andere niet-sleutelkenmerk eigenschap en die mogelijk meer dan één entiteit geretourneerd. De **PartitionKey** waarde identificeert een specifieke partitie en de eigenschap waarden selecteren voor een subset van de entiteiten in de betreffende partitie. Bijvoorbeeld: $filter = PartitionKey eq 'Verkoop' en LastName eq 'Smith'  
* Een ***tabel scannen*** omvat niet de **PartitionKey** en zeer inefficiënte omdat alle van de partities die gezamenlijk uw tabel voor de overeenkomende entiteiten op zijn beurt wordt doorzocht. Wordt uitgevoerd om een tabelscan ongeacht of het filter gebruikt de **RowKey**. Bijvoorbeeld: $filter = LastName eq 'Jones'  
* Query's die meerdere entiteiten retourneren retourneren ze gesorteerd **PartitionKey** en **RowKey** volgorde. Om te voorkomen de entiteiten in de client te sorteren, kies een **RowKey** de meest voorkomende sorteervolgorde te definiëren.  

Houd er rekening mee dat als u met een '**of**' om op te geven van een filter op basis van **RowKey** waarden resulteert in een partitie scan en wordt niet behandeld als een bereikquery. U moet daarom query's die worden gebruikt, zoals filters: $filter = PartitionKey eq 'Sales' (RowKey eq '121' of een RowKey eq '322')  

Zie voor voorbeelden van clientcode die gebruikmaken van de Storage-clientbibliotheek efficiënt query's uitvoeren:  

* [Uitvoeren van een punt-query met behulp van de Storage-clientbibliotheek](#executing-a-point-query-using-the-storage-client-library)
* [Bij het ophalen van meerdere entiteiten met behulp van LINQ](#retrieving-multiple-entities-using-linq)
* [Projectie-serverzijde](#server-side-projection)  

Voor voorbeelden van clientcode dat meerdere Entiteitstypen die zijn opgeslagen in dezelfde tabel kan verwerken, Zie:  

* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

### <a name="choosing-an-appropriate-partitionkey"></a>Een juiste PartitionKey kiezen
Uw keuze van **PartitionKey** moet worden verdeeld moet maakt het gebruik van EGTs (consistentie te garanderen) de vereiste voor de distributie van de entiteiten over meerdere partities (zodat een schaalbare oplossing).  

Op één extreme u alle entiteiten in uw kan opslaan in een enkele partitie, maar dit kan de schaalbaarheid van uw oplossing beperken en zou voorkomen dat de tabelservice kunnen verdelen aanvragen. Het andere uiterste, kunt u één entiteit per partitie, die wel uiterst schaalbare en waardoor de tabelservice op aanvragen voor taakverdeling, maar dat zou voorkomen dat u entiteitstransacties opslaan.  

Een ideaal **PartitionKey** is een die u kunt gebruiken efficiënt query's en met voldoende partities om te controleren of uw oplossing schaalbaar is. Normaal gesproken zult u merken dat uw entiteiten een geschikte eigenschap die de entiteiten over voldoende partities verdeelt hebben.

> [!NOTE]
> Bijvoorbeeld in een systeem dat wordt informatie over gebruikers- of werknemers opgeslagen, gebruikers-id is mogelijk een goede PartitionKey. Mogelijk hebt u verschillende entiteiten die een opgegeven gebruikers-id als de partitiesleutel gebruiken. Elke entiteit die gegevens over een gebruiker opslaat in een enkele partitie worden gegroepeerd en dus deze entiteiten zijn toegankelijk via entiteit groepstransacties, terwijl u nog steeds zeer schaalbaar.
> 
> 

Er zijn aanvullende overwegingen bij de keuze van **PartitionKey** die betrekking hebben op hoe u wilt invoegen, bijwerken en verwijderen van entiteiten: Zie de sectie [ontwerp voor wijziging van gegevens](#design-for-data-modification) hieronder.  

### <a name="optimizing-queries-for-the-table-service"></a>Optimaliseren van query's voor de tabel-service
De tabelservice indexeert automatisch de entiteiten met behulp van de **PartitionKey** en **RowKey** waarden in een enkele geclusterde index, daarom de reden dat wijst u query's zijn het meest efficiënt te gebruiken. Er zijn echter geen indexen dan die op de geclusterde index op de **PartitionKey** en **RowKey**.

Veel ontwerpen moeten voldoen aan de vereisten voor het opzoeken van de entiteiten die zijn gebaseerd op meerdere criteria inschakelen. Bijvoorbeeld zoeken naar werknemer entiteiten op basis van e-mailadres werknemer-id of achternaam op. De volgende patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) deze typen vereiste op te lossen en beschrijven manieren van het feit dat de tabel-service biedt geen secundaire indexen te omzeilen:  

* [Intra-partitie secundaire index patroon](#intra-partition-secondary-index-pattern) -opslaan meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  
* [Secundaire tussen partitioneren van index patroon](#inter-partition-secondary-index-pattern) -opslaan meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden in afzonderlijke partities of in afzonderlijke tabellen snelle en efficiënte zoekacties en alternatieve sorteren inschakelen orders met behulp van verschillende **RowKey** waarden.  
* [Index entiteiten patroon](#index-entities-pattern) -onderhouden index entiteiten zodat efficiënte zoekopdrachten die lijsten van entiteiten retourneren.  

### <a name="sorting-data-in-the-table-service"></a>Sorteren van gegevens in de tabel-service
De tabel-service retourneert entiteiten in oplopende volgorde op basis van gesorteerd **PartitionKey** en vervolgens op **RowKey**. Deze sleutels worden tekenreekswaarden en om ervoor te zorgen dat numerieke waarden correct sorteren, moet u deze converteren naar een vaste lengte en ze worden opgevuld met nullen. Bijvoorbeeld, als de waarde van de werknemer-id die u gebruikt als de **RowKey** is een geheel getal, moet u de werknemer-id converteren **123** naar **00000123**.  

Veel toepassingen hebben vereisten voor het gebruik van gegevens in verschillende volgorden gesorteerd: werknemers bijvoorbeeld sorteren op naam of door datum. De volgende patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) hoe de sorteervolgorde voor uw entiteiten alternatieve adres:  

* [Intra-partitie secundaire index patroon](#intra-partition-secondary-index-pattern) : opslaan van meerdere exemplaren van elke entiteit die gebruikmaken van verschillende RowKey waarden (in dezelfde partitie) waarmee snel en efficiënt zoekacties en alternatieve sorteren orders met behulp van verschillende RowKey waarden.  
* [Secundaire tussen partitioneren van index patroon](#inter-partition-secondary-index-pattern) : opslaan van meerdere exemplaren van elke entiteit met verschillende waarden voor de RowKey in afzonderlijke partities in afzonderlijke tabellen inschakelen snel en efficiënt zoekacties en alternatieve sorteren bestellingen met behulp van verschillende RowKey waarden.
* [Logboek tail patroon](#log-tail-pattern) -ophalen van de  *n*  entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** waarde die in omgekeerde datum en tijd volgorde worden gesorteerd.  

## <a name="design-for-data-modification"></a>Ontwerp voor wijziging van gegevens
Deze sectie richt zich op de ontwerpoverwegingen voor het optimaliseren van toevoegingen, updates, en worden verwijderd. In sommige gevallen moet u de verhouding tussen ontwerpen die optimaliseren voor een query op modellen die voor wijziging van gegevens, optimaliseren net als in ontwerpen voor relationele databases (Hoewel de technieken voor het beheren van de ontwerp-en nadelen andere in een relationele database) evalueren. De sectie [ontwerppatronen voor tabel](#table-design-patterns) beschrijft een aantal gedetailleerde ontwerppatronen voor de tabel-service en worden enkele deze verschillen. In de praktijk vindt u veel ontwerpen die zijn geoptimaliseerd voor het uitvoeren van query's entiteiten ook geschikt voor entiteiten wijzigen.  

### <a name="optimizing-the-performance-of-insert-update-and-delete-operations"></a>Optimaliseren van de prestaties van invoegen, bijwerken en verwijderen van bewerkingen
Als u wilt bijwerken of verwijderen van een entiteit, moet u kunnen worden geïdentificeerd met behulp van de **PartitionKey** en **RowKey** waarden. In dit opzicht van uw keuze **PartitionKey** en **RowKey** voor entiteiten wijzigen dezelfde criteria voor uw keuze volgen moet voor de ondersteuning van punt query's omdat u wilt identificeren entiteiten zo efficiënt mogelijk. U niet wilt dat een entiteit Zoek om te detecteren met behulp van een inefficiënte partitie of tabel scan de **PartitionKey** en **RowKey** waarden die u wilt bijwerken of verwijderen.  

De volgende patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) adres optimaliseert de prestaties of de insert, update en verwijderbewerkingen:  

* [Hoog volume patroon verwijderen](#high-volume-delete-pattern) -de verwijdering van een groot aantal entiteiten inschakelen door het opslaan van alle entiteiten voor gelijktijdige verwijdering in hun eigen afzonderlijke tabel; u de entiteiten verwijderen door de tabel verwijderen.  
* [Patroon van de reeks gegevens](#data-series-pattern) -Store voltooid gegevensreeksen in één entiteit om te beperken het aantal aanvragen die u aanbrengt.  
* [Wide entiteiten patroon](#wide-entities-pattern) -gebruik van meerdere fysieke entiteiten voor het opslaan van logische entiteiten met meer dan 252 eigenschappen.  
* [Grote entiteiten patroon](#large-entities-pattern) -blob storage gebruiken voor het opslaan van grote eigenschapswaarden.  

### <a name="ensuring-consistency-in-your-stored-entities"></a>Uw opgeslagen entiteiten consistent te houden
De andere belangrijke factoren die van invloed is op uw keuze van sleutels voor het optimaliseren van gegevenswijzigingen is hoe u de consistentie met behulp van atomische transacties. U kunt alleen een EGT bewerkingen uitvoeren op entiteiten die zijn opgeslagen in dezelfde partitie.  

De volgende patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) adres consistentie beheren:  

* [Intra-partitie secundaire index patroon](#intra-partition-secondary-index-pattern) -opslaan meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  
* [Secundaire tussen partitioneren van index patroon](#inter-partition-secondary-index-pattern) : opslaan van meerdere exemplaren van elke entiteit die gebruikmaken van verschillende waarden voor de RowKey in afzonderlijke partities of in afzonderlijke tabellen waarmee snel en efficiënt zoekacties en alternatieve sorteren orders met behulp van verschillende **RowKey** waarden.  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) -uiteindelijk consistent gedrag over grenzen van partities of opslag system grenzen inschakelen met behulp van Azure wachtrijen.
* [Index entiteiten patroon](#index-entities-pattern) -onderhouden index entiteiten zodat efficiënte zoekopdrachten die lijsten van entiteiten retourneren.  
* [Denormalization patroon](#denormalization-pattern) -combineren van de bijbehorende gegevens samen in één entiteit waarmee u kunt alle gegevens die u nodig hebt met een query één punt ophalen.  
* [Patroon van de reeks gegevens](#data-series-pattern) -Store voltooid gegevensreeksen in één entiteit om te beperken het aantal aanvragen die u aanbrengt.  

Zie de sectie voor informatie over entiteit groepstransacties [entiteit groepstransacties](#entity-group-transactions).  

### <a name="ensuring-your-design-for-efficient-modifications-facilitates-efficient-queries"></a>Uw ontwerp voor efficiënte wijzigingen gezorgd vereenvoudigt efficiënt query 's
Een ontwerp voor een efficiënte query resulteert in efficiënt wijzigingen, maar u moet altijd in veel gevallen evalueren of dit het geval is voor uw specifieke scenario. Sommige van de patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) expliciet evalueren verschillen tussen het uitvoeren van query's en entiteiten wijzigen en u moet altijd rekening gehouden met het nummer van elk type bewerking.  

De volgende patronen in de sectie [ontwerppatronen voor tabel](#table-design-patterns) adres verschillen tussen ontwerpen voor efficiënt query's en ontwerpen voor wijziging van efficiënte gegevens:  

* [Samengestelde sleutelpatroon](#compound-key-pattern) -gebruik samengestelde **RowKey** waarden om in te schakelen van een client voor het opzoeken van gerelateerde gegevens met een query één punt.  
* [Logboek tail patroon](#log-tail-pattern) -ophalen van de  *n*  entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** waarde die in omgekeerde datum en tijd volgorde worden gesorteerd.  

## <a name="encrypting-table-data"></a>Versleuteling van tabelgegevens
De .NET Azure Storage-clientbibliotheek biedt ondersteuning voor versleuteling van entiteitseigenschappen tekenreeks voor invoegen en vervang bewerkingen. De versleutelde tekenreeksen worden opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na de decodering.    

Gebruikers moeten de eigenschappen moeten worden gecodeerd opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door een kenmerk [EncryptProperty] (voor POCO-entiteiten die zijn afgeleid van TableEntity) of een omzetter versleuteling in de aanvraag-opties. Een oplossing versleuteling is een gemachtigde die ervoor zorgt dat een partitiesleutel, rijsleutel en de naam van eigenschap retourneert een Booleaanse waarde die aangeeft of de eigenschap die moet worden versleuteld. Tijdens het versleutelen gebruik de clientbibliotheek van deze informatie om te bepalen of een eigenschap tijdens het schrijven naar de kabel moet worden versleuteld. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld als X, vervolgens versleutelen eigenschap A; anders versleutelen eigenschappen A en B.) Houd er rekening mee dat het is niet nodig om deze informatie tijdens het lezen of het uitvoeren van query's entiteiten te geven.

Houd er rekening mee dat samenvoegen wordt momenteel niet ondersteund. Omdat een subset van eigenschappen mogelijk zijn versleuteld eerder met een andere sleutel, leidt gewoon de nieuwe eigenschappen samenvoegen en bijwerken van de metagegevens tot verlies van gegevens. Samenvoegen van een vereist extra service aanroepen om te lezen van de vooraf bestaande entiteit van de service of met een nieuwe sleutel per eigenschap, die beide zijn niet geschikt voor betere prestaties.     

Zie voor meer informatie over het coderen van tabelgegevens [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../storage/common/storage-client-side-encryption.md).  

## <a name="modelling-relationships"></a>Modellering van relaties
Ontwikkelen van domeinmodellen is een belangrijke stap in het ontwerp van complexe systemen. U kunt gewoonlijk modellen proces gebruiken om entiteiten en de relaties tussen deze als een manier om te begrijpen van het domein voor bedrijven en informeren over het ontwerp van uw systeem te identificeren. Deze sectie richt zich op hoe Vertaal enkele van de algemene relatietypen gevonden in domeinmodellen ontwerpen voor de tabel-service. Het proces van toewijzing van een logische gegevensmodel aan een fysieke basis NoSQL-gegevensmodel is heel verschillend van die gebruikt bij het ontwerpen van een relationele database. Relationele databases ontwerp neemt doorgaans een gegevens-normalisatie-proces geoptimaliseerd voor het minimaliseren van redundantie – en een declaratieve opvragen capaciteit die isoleert hoe de uitvoering van hoe u de database werkt.  

### <a name="one-to-many-relationships"></a>Een-op-veel-relaties
Een-op-veel-relaties tussen bedrijven domeinobjecten heel vaak gebeuren: één afdeling heeft bijvoorbeeld veel werknemers. Er zijn verschillende manieren voor het implementeren van een-op-veel-relaties in de tabelservice elke met de voor- en nadelen die relevant voor een bepaald scenario zijn mogelijk.  

Bekijk het voorbeeld van een grote onderneming met meerdere nationale met tienduizenden afdelingen en werknemer entiteiten waar elke afdeling heeft veel werknemers en elke werknemer die gekoppeld is aan een bepaalde afdeling. Een aanpak is het opslaan van afzonderlijke afdeling en entiteiten van de werknemer zoals deze:  

![][1]

Dit voorbeeld ziet u een impliciete een-op-veel-relatie tussen de typen op basis van de **PartitionKey** waarde. Elke afdeling kan veel werknemers hebben.  

In dit voorbeeld toont ook een entiteit afdeling en de gerelateerde werknemer entiteiten in dezelfde partitie. U kunt verschillende partities, tabellen of zelfs storage-accounts gebruiken voor de verschillende entiteittypen.  

Er is een alternatieve methode denormalize van uw gegevens en store-alleen werknemer entiteiten met gedenormaliseerd afdelingsgegevens zoals weergegeven in het volgende voorbeeld. In dit specifieke scenario gedenormaliseerd hiervan mogelijk niet het beste als er een vereiste kunnen de details van een afdelingsmanager niet wijzigen omdat hiervoor moet u elke werknemer van de afdeling bijwerken.  

![][2]

Zie voor meer informatie de [Denormalization patroon](#denormalization-pattern) verderop in deze handleiding.  

De volgende tabel geeft een overzicht van de voordelen en nadelen van elk van de methoden die hierboven worden beschreven voor het opslaan van de werknemer en afdeling entiteiten met een-op-veel-relatie een. U moet ook overwegen hoe vaak verwacht u dat kunt u verschillende bewerkingen uitvoeren: kan het zijn aanvaardbaar is voor een ontwerp waarin een dure bewerking als die voor deze bewerking alleen zelden gebeurt hebben.  

<table>
<tr>
<th>Benadering</th>
<th>Professionals</th>
<th>Nadelen</th>
</tr>
<tr>
<td>Afzonderlijke Entiteitstypen, dezelfde partitie, dezelfde tabel</td>
<td>
<ul>
<li>U kunt een entiteit afdeling bijwerken met één bewerking.</li>
<li>U kunt een EGT om consistentie te behouden als u een vereiste voor het wijzigen van een entiteit afdeling hebt wanneer u update, invoegen, verwijderen een werknemer entiteit. Bijvoorbeeld als u een aantal afdelingen medewerkers voor elke afdeling onderhouden.</li>
</ul>
</td>
<td>
<ul>
<li>Mogelijk moet u een entiteit van de afdeling voor sommige activiteiten van de client en een werknemer worden opgehaald.</li>
<li>Opslagbewerkingen gebeuren op dezelfde partitie. Hoge transactie volumes, kan dit resulteren in een hotspot.</li>
<li>U kunt een werknemer niet verplaatsen naar een nieuwe afdeling met behulp van een EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Afzonderlijke Entiteitstypen, verschillende partities of tabellen of storage-accounts</td>
<td>
<ul>
<li>U kunt een entiteit van de afdeling of de werknemer entiteit bijwerken met één bewerking.</li>
<li>Op hoog transactie volumes, kan dit helpen de werklast verdeeld over meer partities.</li>
</ul>
</td>
<td>
<ul>
<li>Mogelijk moet u een entiteit van de afdeling voor sommige activiteiten van de client en een werknemer worden opgehaald.</li>
<li>U kunt EGTs niet gebruiken om consistentie te behouden wanneer u update, invoegen, verwijderen van een werknemer en update een afdeling. Een aantal werknemers in een entiteit afdeling bijvoorbeeld wordt bijgewerkt.</li>
<li>U kunt een werknemer niet verplaatsen naar een nieuwe afdeling met behulp van een EGT.</li>
</ul>
</td>
</tr>
<tr>
<td>Denormalize in één entiteitstype</td>
<td>
<ul>
<li>U kunt alle informatie die u nodig hebt met één aanvraag ophalen.</li>
</ul>
</td>
<td>
<ul>
<li>Het is mogelijk dure om consistentie te behouden als u bijwerken afdelingsgegevens wilt (dit vereist bij te werken van alle werknemers in een afdeling).</li>
</ul>
</td>
</tr>
</table>

Hoe u kiezen tussen deze opties en welke van de voor- en nadelen zijn de meest significante, is afhankelijk van uw specifieke toepassingsscenario's. Bijvoorbeeld, hoe vaak u Wijzig afdeling entiteiten; moeten alle werknemers-query's de afdelingen aanvullende informatie; hoe sluiten weet u de limieten voor schaalbaarheid van de partities of uw storage-account?  

### <a name="one-to-one-relationships"></a>-Op-een-relaties
Domeinmodellen kunnen-op-een-relaties tussen entiteiten bevatten. Als u nodig hebt voor het implementeren van een-op-een relatie in de tabel-service, moet u ook het koppelen van de twee gerelateerde entiteiten wanneer moet u beide ophalen. Deze koppeling kan impliciete, op basis van een overeenkomst in de sleutelwaarden of expliciete worden door het opslaan van een koppeling in de vorm van **PartitionKey** en **RowKey** waarden in elke entiteit naar de gerelateerde entiteit. Zie de sectie voor een bespreking van of moet u de gerelateerde entiteiten opslaan op dezelfde partitie [een-op-veel relaties](#one-to-many-relationships).  

Houd er rekening mee dat er zijn ook overwegingen bij de implementatie die kunnen leiden bij de implementatie-op-een-relaties in de tabel-service:  

* Verwerking van grote entiteiten (Zie voor meer informatie [grote entiteiten patroon](#large-entities-pattern)).  
* Toegangsbeheer implementeren (Zie voor meer informatie [beheren van toegang met Shared Access Signatures](#controlling-access-with-shared-access-signatures)).  

### <a name="join-in-the-client"></a>Deelnemen aan de client
Hoewel er manieren om relaties in de tabel-service, moet u niet vergeet dat de twee belangrijkste redenen voor het gebruik van de tabelservice schaalbaarheid en prestaties zijn. Als u dat u veel-relaties die een bedreiging vormen van de prestaties en schaalbaarheid van uw oplossing zijn modellering vindt, vraagt u uzelf als het is nodig om alle gegevensrelaties in uw tabelontwerp samen te stellen. U kunt mogelijk het ontwerp te vereenvoudigen en verbeteren van de schaalbaarheid en prestaties van uw oplossing als u toestaat dat de clienttoepassing alle benodigde joins uitvoeren.  

Bijvoorbeeld, als er kleine tabellen die gegevens bevatten die niet vaak veranderen, kunt vervolgens u deze gegevens eenmaal ophalen en cache te plaatsen op de client. Dit kunt voorkomen dat herhaalde interactie om op te halen van dezelfde gegevens. In de voorbeelden die we in deze handleiding hebt bekeken, heeft de reeks afdelingen in een kleine organisatie waarschijnlijk klein en wijzig zelden waardoor het een goede kandidaat voor de gegevens eenmaal in client-toepassing kan worden gedownload en cache als opzoeken van gegevens.  

### <a name="inheritance-relationships"></a>Relaties voor overname
Als u de clienttoepassing gebruikmaakt van een set klassen die deel uitmaken van een overnamerelatie bedrijfsentiteiten vertegenwoordigt, kunt u eenvoudig deze entiteiten in de tabel-service behouden. Bijvoorbeeld, u wellicht de volgende set van klassen die zijn gedefinieerd in uw clienttoepassing waar **persoon** is een abstracte klasse.

![][3]

U kunt deze persistent maken exemplaren van de twee concrete klassen in de tabel-service met één personentabel met entiteiten in die zijn opgemaakt als volgt:  

![][4]

Zie de sectie voor meer informatie over het werken met meerdere Entiteitstypen in dezelfde tabel in de clientcode [werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types) verderop in deze handleiding. Dit vindt u voorbeelden van het herkennen van het entiteitstype in de clientcode.  

## <a name="table-design-patterns"></a>Ontwerppatronen voor tabel
U hebt gezien gedetailleerde discussies over het optimaliseren van uw tabelontwerp voor beide entiteitsgegevens op te halen met behulp van query's en voor het invoegen, bijwerken en verwijderen van entiteitsgegevens in de vorige secties. Deze sectie beschrijft een aantal patronen geschikt voor gebruik met oplossingen voor tabel-service. Bovendien ziet u hoe u enkele van de problemen en een afweging maken wat eerder in deze handleiding worden gegenereerd op vrijwel kunt oplossen. Het volgende diagram geeft een overzicht van de relaties tussen de verschillende patronen:  

![][5]

De kaart patroon hierboven licht sommige relaties tussen patronen (blauw) en anti patronen (oranje) die in deze handleiding worden beschreven. Er zijn natuurlijk veel andere patronen die moeten overwogen worden. Bijvoorbeeld, een van de belangrijkste scenario's voor tabel-Service is met de [gematerialiseerd weergave patroon](https://msdn.microsoft.com/library/azure/dn589782.aspx) van de [opdracht Query verantwoordelijkheid scheiding (CQRS)](https://msdn.microsoft.com/library/azure/jj554200.aspx) patroon.  

### <a name="intra-partition-secondary-index-pattern"></a>Intra-partitie secundaire index patroon
Opslaan van meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden (in dezelfde partitie) inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden. Updates tussen kopieën consistent kunnen worden gehouden met behulp van EGT.  

#### <a name="context-and-problem"></a>Context en probleem
De tabelservice indexeert automatisch entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing voor het ophalen van een entiteit efficiënt gebruik maakt van deze waarden. Bijvoorbeeld, met de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kunt gebruiken een punt-query voor een afzonderlijke werknemer entiteit ophalen met de afdelingsnaam van de en de werknemer-id (de **PartitionKey** en **RowKey** waarden). Een client kan ook gesorteerd op werknemer-id binnen elke afdeling entiteiten ophalen.

![][6]

Als u ook kunnen wilt vinden van een werknemer entiteit op basis van de waarde van een andere eigenschap, zoals e-mailadres, moet u een minder efficiënte partitie scan te vinden. Dit is omdat de tabelservice geen secundaire indexen biedt. Er is bovendien geen optie voor het aanvragen van een lijst met werknemers gesorteerd in een andere volgorde dan **RowKey** volgorde.  

#### <a name="solution"></a>Oplossing
U kunt het ontbreken van een secundaire indexen omzeilen, kunt u meerdere exemplaren van elke entiteit bij elk exemplaar met een andere opslaan **RowKey** waarde. Als u een entiteit met de onderstaande structuren opslaat, kunt u efficiënt werknemer entiteiten op basis van e-adres of de werknemer-id ophalen. Het voorvoegsel waarden voor de **RowKey**, 'empid_' en 'email_', kunt u zoeken naar één werknemer of een bereik van werknemers met behulp van een bereik van e-mailadressen of werknemer-id's.  

![][7]

De volgende twee filtercriteria (één opzoeken op basis van de werknemer-id en een opzoeken op basis van e-mailadres) Geef beide punt query's:  

* $filter = (PartitionKey eq 'Sales') en (RowKey eq 'empid_000223')  
* $filter = (PartitionKey eq 'Sales') en (RowKey eq 'email_jonesj@contoso.com')  

Als u een query voor een bereik van de werknemer entiteiten uitvoert, kunt u een bereik gesorteerd in volgorde van de werknemer-id of een bereik in e-mailadres volgorde gesorteerd op basis van een query uitvoert voor entiteiten met het juiste voorvoegsel op in de **RowKey**.  

* Alle medewerkers vinden in de afdeling verkoop met een werknemer-id in het bereik 000100-000199 gebruik: $filter = (PartitionKey eq 'Sales') en (RowKey ge 'empid_000100') en (RowKey RP 'empid_000199')  
* Alle medewerkers van de verkoopafdeling vinden met een e-mailadres begint met de letter "a" gebruik: $filter = (PartitionKey eq 'Sales') en (RowKey ge 'email_a') en (RowKey lt 'email_b')  
  
  Opmerking: de filtersyntaxis gebruikt in de bovenstaande voorbeelden uit de tabelservice REST API, Zie voor meer informatie is [Query entiteiten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Table storage is relatief goedkope gebruiken zodat de overhead van de kosten voor het opslaan van dubbele gegevens mag geen groot belang. U moet echter altijd de kosten van het ontwerp op basis van de verwachte opslagvereisten geëvalueerd en alleen toevoegen dubbele entiteiten ter ondersteuning van de query's die de clienttoepassing wordt uitgevoerd.  
* Omdat de secundaire index entiteiten worden opgeslagen in dezelfde partitie als de oorspronkelijke entiteiten, moet u ervoor zorgen dat u de schaalbaarheidsdoelen voor een afzonderlijke partitie niet overschrijden.  
* U kunt uw dubbele entiteiten consistent zijn met elkaar houden met behulp van EGTs moment bijwerken van de twee kopieën van de entiteit. Dit betekent dat alle exemplaren van een entiteit in dezelfde partitie moeten worden opgeslagen. Zie voor meer informatie de sectie [entiteitstransacties met behulp van](#entity-group-transactions).  
* De waarde voor de **RowKey** moet uniek zijn voor elke entiteit. Overweeg het gebruik van de samengestelde sleutel komt.  
* Opvulling numerieke waarden in de **RowKey** (bijvoorbeeld de werknemer-id 000223), kunt sorteren en filteren op basis van hoofdletters en de ondergrenzen corrigeren.  
* U hoeft hoeft niet te dupliceren van de eigenschappen van de entiteit. Bijvoorbeeld, als de query's die lookup de entiteiten die u het e-adres in de **RowKey** nooit moet de leeftijd van de werknemer, deze entiteiten kunnen de volgende structuur:

![][8]

* Is het doorgaans beter dubbele gegevens opslaan en ervoor te zorgen dat u alle gegevens die u nodig hebt met een enkele query kunt ophalen dan het gebruik van een query naar een entiteit en een andere voor het opzoeken van de vereiste gegevens.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Gebruik dit patroon wanneer uw clienttoepassing moet met een aantal verschillende sleutels wanneer de client hoeft op te halen van entiteiten in een andere sorteervolgorde entiteiten ophalen en waar u elke entiteit met een aantal unieke waarden kunt identificeren. U moet echter zeker van te zijn dat de limieten voor schaalbaarheid van partitie niet te overschrijden wanneer u de entiteit zoekacties met behulp van de verschillende uitvoert **RowKey** waarden.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Secundaire tussen partitioneren van index patroon](#inter-partition-secondary-index-pattern)
* [Samengestelde sleutel patroon](#compound-key-pattern)
* [Entiteit groep transacties](#entity-group-transactions)
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)

### <a name="inter-partition-secondary-index-pattern"></a>Secundaire tussen partitioneren van index patroon
Opslaan van meerdere exemplaren van elke entiteit met behulp van verschillende **RowKey** waarden in afzonderlijke partities of in afzonderlijke tabellen inschakelen snel en efficiënt zoekacties en alternatieve sorteervolgorde met behulp van verschillende **RowKey** waarden.  

#### <a name="context-and-problem"></a>Context en probleem
De tabelservice indexeert automatisch entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Dit kan een clienttoepassing voor het ophalen van een entiteit efficiënt gebruik maakt van deze waarden. Bijvoorbeeld, met de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kunt gebruiken een punt-query voor een afzonderlijke werknemer entiteit ophalen met de afdelingsnaam van de en de werknemer-id (de **PartitionKey** en **RowKey** waarden). Een client kan ook gesorteerd op werknemer-id binnen elke afdeling entiteiten ophalen.  

![][9]

Als u ook kunnen wilt vinden van een werknemer entiteit op basis van de waarde van een andere eigenschap, zoals e-mailadres, moet u een minder efficiënte partitie scan te vinden. Dit is omdat de tabelservice geen secundaire indexen biedt. Er is bovendien geen optie voor het aanvragen van een lijst met werknemers gesorteerd in een andere volgorde dan **RowKey** volgorde.  

U bent anticiperen op een zeer groot aantal transacties tegen deze entiteiten en wilt de risico's van de tabelservice beperking van de client.  

#### <a name="solution"></a>Oplossing
U kunt het ontbreken van een secundaire indexen omzeilen, kunt u meerdere exemplaren van elke entiteit met het gebruik van elk exemplaar andere opslaan **PartitionKey** en **RowKey** waarden. Als u een entiteit met de onderstaande structuren opslaat, kunt u efficiënt werknemer entiteiten op basis van e-adres of de werknemer-id ophalen. Het voorvoegsel waarden voor de **PartitionKey**, 'empid_' en 'email_' kunt u bepalen welke index die u wilt gebruiken voor een query.  

![][10]

De volgende twee filtercriteria (één opzoeken op basis van de werknemer-id en een opzoeken op basis van e-mailadres) Geef beide punt query's:  

* $filter = (PartitionKey eq ' empid_Sales') en (RowKey eq '000223')
* $filter = (PartitionKey eq ' email_Sales') en (RowKey eq 'jonesj@contoso.com')  

Als u een query voor een bereik van de werknemer entiteiten uitvoert, kunt u een bereik gesorteerd in volgorde van de werknemer-id of een bereik in e-mailadres volgorde gesorteerd op basis van een query uitvoert voor entiteiten met het juiste voorvoegsel op in de **RowKey**.  

* Alle medewerkers vinden in de afdeling verkoop met een werknemer-id in het bereik **000100** naar **000199** in gebruik door werknemers-id volgorde gesorteerd: $filter = (PartitionKey eq ' empid_Sales') en (RowKey ge '000100') en (RowKey RP '000199')  
* Alle medewerkers vinden in de afdeling verkoop met een e-mailadres dat met "a" gesorteerde e-mailadres volgorde gebruikt begint: $filter = (PartitionKey eq ' email_Sales') en (RowKey ge "a") en (RowKey lt "b")  

Opmerking: de filtersyntaxis gebruikt in de bovenstaande voorbeelden uit de tabelservice REST API, Zie voor meer informatie is [Query entiteiten](http://msdn.microsoft.com/library/azure/dd179421.aspx).  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* U uw dubbele entiteiten uiteindelijk consistent is met elkaar kunt houden met behulp van de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) onderhouden van de primaire en secundaire index-entiteiten.  
* Table storage is relatief goedkope gebruiken zodat de overhead van de kosten voor het opslaan van dubbele gegevens mag geen groot belang. U moet echter altijd de kosten van het ontwerp op basis van de verwachte opslagvereisten geëvalueerd en alleen toevoegen dubbele entiteiten ter ondersteuning van de query's die de clienttoepassing wordt uitgevoerd.  
* De waarde voor de **RowKey** moet uniek zijn voor elke entiteit. Overweeg het gebruik van de samengestelde sleutel komt.  
* Opvulling numerieke waarden in de **RowKey** (bijvoorbeeld de werknemer-id 000223), kunt sorteren en filteren op basis van hoofdletters en de ondergrenzen corrigeren.  
* U hoeft hoeft niet te dupliceren van de eigenschappen van de entiteit. Bijvoorbeeld, als de query's die lookup de entiteiten die u het e-adres in de **RowKey** nooit moet de leeftijd van de werknemer, deze entiteiten kunnen de volgende structuur:
  
  ![][11]
* Is het doorgaans beter dubbele gegevens opslaan en ervoor te zorgen dat u alle gegevens die u nodig hebt met één query dan te gebruiken één query vinden van een entiteit met behulp van de secundaire index en een andere lookup de vereiste gegevens in de primaire index kunt ophalen.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Gebruik dit patroon wanneer uw clienttoepassing moet met een aantal verschillende sleutels wanneer de client hoeft op te halen van entiteiten in een andere sorteervolgorde entiteiten ophalen en waar u elke entiteit met een aantal unieke waarden kunt identificeren. Dit patroon gebruiken wanneer u voorkomen dat de partitie-limieten voor schaalbaarheid dan wilt wanneer u de entiteit zoekacties met behulp van de verschillende uitvoert **RowKey** waarden.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Intra-partitie secundaire index patroon](#intra-partition-secondary-index-pattern)  
* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

### <a name="eventually-consistent-transactions-pattern"></a>Uiteindelijk consistent transacties patroon
Uiteindelijk consistent gedrag over grenzen van partities of opslag system grenzen inschakelen met behulp van Azure wachtrijen.  

#### <a name="context-and-problem"></a>Context en probleem
EGTs inschakelen atomische transacties tussen meerdere entiteiten die dezelfde partitiesleutel delen. Voor betere prestaties en schaalbaarheid u besluiten voor het opslaan van entiteiten met vereisten voor consistentie in afzonderlijke partities of in een afzonderlijke opslagsysteem: in een dergelijk scenario u EGTs niet gebruiken voor het handhaven van de consistentie. Zo mogelijk een vereiste uiteindelijke consistentie tussen onderhouden:  

* Entiteiten in opgeslagen in twee verschillende partities in dezelfde tabel in verschillende tabellen in verschillende opslagaccounts.  
* Een entiteit die is opgeslagen in de tabel-service en een blob die is opgeslagen in de Blob-service.  
* Een entiteit in de tabel-service en een bestand opgeslagen in een bestandssysteem.  
* Een entiteit store in de tabel-service nog geïndexeerd met de Azure Search-service.  

#### <a name="solution"></a>Oplossing
U kunt een oplossing die zorgt voor uiteindelijke consistentie tussen twee of meer partities of opslagsystemen implementeren met behulp van Azure wachtrijen.
Ter illustratie van deze benadering wordt ervan uitgegaan dat u hebt een vereiste kunnen archiveren oude werknemer entiteiten. Oude werknemer entiteiten zelden worden opgevraagd en van alle activiteiten die betrekking op de huidige werknemers hebben moeten worden uitgesloten. Voor het implementeren van deze vereiste actieve werknemers op te slaan de **huidige** tabel en oude werknemers in de **archief** tabel. Een werknemer archiveren, moet u verwijderen van de entiteit van de **huidige** tabel en het toevoegen van de entiteit de **archief** tabel, maar u een EGT niet gebruiken voor het uitvoeren van deze twee bewerkingen. Om te voorkomen dat het risico dat een fout een entiteit veroorzaakt moet worden weergegeven in beide of geen van beide tabellen, moet de archiveringsbewerking uiteindelijk consistent is. Het volgende diagram van de takenreeks bevat de stappen in deze bewerking. Meer details wordt uitzondering paden in de volgende tekst geleverd.  

![][12]

Een client initieert de archiveringsbewerking door het plaatsen van een bericht op een Azure-wachtrij in dit voorbeeld voor het archiveren van de werknemer #456. Een werkrol, peilt de wachtrij voor nieuwe berichten; Wanneer er een is gevonden, leest het bericht en wordt een verborgen kopie in de wachtrij. De werkrol een kopie van de entiteit van de volgende haalt de **huidige** tabel, voegt u een kopie in het **archief** tabel en verwijdert u vervolgens de oorspronkelijke uit de **huidige** tabel. Ten slotte, als er geen fouten uit de vorige stap zijn, de werkrol verborgen wordt het bericht verwijderd uit de wachtrij.  

In dit voorbeeld voegt stap 4 van de werknemer in het **archief** tabel. Dit kan de werknemer toevoegen aan een blob in de Blob-service of een bestand in een bestandssysteem.  

#### <a name="recovering-from-failures"></a>Herstellen van fouten
Het is belangrijk dat de bewerkingen in stappen **4** en **5** moet *idempotent* voor het geval de werkrol opnieuw opstarten van de archiveringsbewerking. Als u de tabelservice voor stap **4** moet u een bewerking "invoegen of vervangen"; voor de stap **5** moet u een ' verwijderen als bestaat ' bewerking in de clientbibliotheek die u gebruikt. Als u een andere opslagsysteem gebruikt, moet u een geschikte idempotent-bewerking.  

Als de werkrol nooit stap voltooit **6**, en vervolgens verschijnt het bericht opnieuw na een time-out voor de wachtrij gereed is voor de werkrol proberen te deze opnieuw te verwerken. De werkrol kunt controleren hoe vaak een bericht in de wachtrij is gelezen en, indien nodig, markeren is een 'poison'-bericht voor onderzoek door ze naar een afzonderlijke wachtrij. Zie voor meer informatie over het lezen van berichten in wachtrij plaatsen en het controleren van de wachtrij halen telling [berichten ophalen](https://msdn.microsoft.com/library/azure/dd179474.aspx).  

Er zijn fouten van de tabel en Queue-services zijn tijdelijke fouten en uw clienttoepassing bevatten geschikte Pogingslogica om deze te verwerken.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Deze oplossing biedt geen voor het isoleren van de transactie. Bijvoorbeeld, een client kan lezen de **huidige** en **archief** tabellen wanneer de werkrol werd tussen stappen **4** en **5**, en een inconsistente weergave van de gegevens ziet. Houd er rekening mee dat de gegevens zal consistent uiteindelijk.  
* U moet ervoor dat de stappen 4 en 5 idempotent zijn om de uiteindelijke consistentie te garanderen.  
* U kunt de oplossing met behulp van meerdere wachtrijen en rolinstanties worker schalen.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Gebruik dit patroon als u wilt garanderen uiteindelijke consistentie tussen entiteiten die aanwezig zijn in verschillende partities of tabellen. U kunt dit patroon om ervoor te zorgen uiteindelijke consistentie voor bewerkingen in de tabel-service en de Blob-service en andere Azure Storage gegevensbronnen zoals database of het bestandssysteem uitbreiden.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)  
* [Samenvoegen of vervangen](#merge-or-replace)  

> [!NOTE]
> Als de transactie-isolatieniveau is belangrijk dat uw oplossing, kunt u overwegen opnieuw ontwerpen van uw tabellen zodat u EGTs gebruiken.  
> 
> 

### <a name="index-entities-pattern"></a>Index entiteiten patroon
Onderhouden index entiteiten zodat efficiënte zoekopdrachten die lijsten van entiteiten retourneren.  

#### <a name="context-and-problem"></a>Context en probleem
De tabelservice indexeert automatisch entiteiten met behulp van de **PartitionKey** en **RowKey** waarden. Hierdoor wordt een clienttoepassing voor het ophalen van een entiteit efficiënt gebruik maakt van een punt-query. Bijvoorbeeld, met behulp van de structuur van de tabel hieronder wordt weergegeven, een clienttoepassing kunt efficiënt een afzonderlijke werknemer entiteit ophalen met behulp van de naam van de afdeling en de werknemer-id (de **PartitionKey** en **RowKey**).  

![][13]

Als u ook kunnen wilt ophalen van een lijst van de werknemer entiteiten op basis van de waarde van een andere niet-unieke eigenschap, zoals hun achternaam, moet u een minder efficiënte partitie scan om te zoeken naar overeenkomsten in plaats van een index met ze rechtstreeks opzoeken. Dit is omdat de tabelservice geen secundaire indexen biedt.  

#### <a name="solution"></a>Oplossing
Als u wilt zoeken op naam inschakelen met de structuur van de entiteit hierboven weergegeven, moet u een lijst met werknemer-id's onderhouden. Als u wilt ophalen van de werknemer entiteiten met een bepaalde achternaam, zoals Peeters, moet u eerst de lijst met werknemer-id vinden voor werknemers met Jones als hun achternaam en vervolgens deze werknemer entiteiten worden opgehaald. Er zijn drie manieren voor het opslaan van de lijsten met werknemer-id's:  

* Blob storage gebruiken.  
* Index entiteiten in dezelfde partitie als de werknemer-entiteiten maken.  
* Maak index entiteiten in een afzonderlijke partitie of de tabel.  

<u>Optie #1: Gebruik blob storage</u>  

Voor de eerste optie, u een blob voor elke unieke achternaam en in elke blob-store een lijst maken van de **PartitionKey** (afdeling) en **RowKey** (werknemer-id) waarden voor werknemers die die achternaam hebben. Bij het toevoegen of verwijderen van een werknemer moet u ervoor zorgen dat de inhoud van de relevante blob uiteindelijk consistent is met de werknemer entiteiten is.  

<u>Optie #2:</u> index entiteiten in dezelfde partitie maken  

Gebruik voor de tweede optie, index-entiteiten die de volgende gegevens worden opgeslagen:  

![][14]

De **EmployeeIDs** eigenschap bevat een lijst van de werknemer-id's voor werknemers met de achternaam opgeslagen in de **RowKey**.  

De volgende stappen wordt beschreven hoe die u volgen moet wanneer u een nieuwe werknemer toevoegen wilt als u de tweede optie. In dit voorbeeld toevoegen we een werknemer met Id 000152 en een achternaam Jones van de verkoopafdeling:  

1. Ophalen van de entiteit index met een **PartitionKey** waarde 'Verkoop' en de **RowKey** waarde "Jones." Sla de ETag van deze entiteit moet worden gebruikt in stap 2.  
2. Maken van een entiteit groep transactie (dat wil zeggen, een batchbewerking) die de nieuwe werknemer entiteit ingevoegd (**PartitionKey** waarde 'Verkoop' en **RowKey** waarde '000152'), en updates van de entiteit index (**PartitionKey** waarde 'Verkoop' en **RowKey** waarde 'Jones') door de nieuwe werknemers-id toe te voegen aan de lijst in het veld EmployeeIDs. Zie voor meer informatie over entiteit groepstransacties [entiteit groepstransacties](#entity-group-transactions).  
3. Als de entiteit groep transactie is mislukt vanwege een optimistische gelijktijdigheid-fout (iemand anders heeft alleen de entiteit index gewijzigd), moet u opnieuw beginnen bij stap 1.  

U kunt een soortgelijke benadering van een werknemer verwijderen als u de tweede optie gebruiken. Wijzigen van de achternaam van een werknemer is iets ingewikkelder omdat u moet uitvoeren van een entiteit groep transactie die drie entiteiten worden bijgewerkt: de entiteit van de werknemer, de entiteit van de index voor de achternaam van de oude en de entiteit van de index voor de nieuwe achternaam. Voordat u wijzigingen aanbrengt in om op te halen van de ETag-waarden die u vervolgens gebruiken kunt voor het uitvoeren van de updates gebruikt optimistische gelijktijdigheid, moet u elke entiteit ophalen.  

De volgende stappen wordt beschreven hoe die u volgen moet wanneer u alle werknemers met een bepaalde achternaam in een afdeling opzoeken moet, als u de tweede optie. In dit voorbeeld zijn er opzoeken van alle werknemers met een achternaam Jones in de afdeling verkoop:  

1. Ophalen van de entiteit index met een **PartitionKey** waarde 'Verkoop' en de **RowKey** waarde "Jones."  
2. De lijst met werknemer-id's in het veld EmployeeIDs parseren.  
3. Als u meer informatie over elk van deze werknemers (zoals hun e-mailadressen), ophalen van elk van de werknemer entiteiten met **PartitionKey** waarde 'Verkoop' en **RowKey** waarden uit de lijst met werknemers die u hebt verkregen in stap 2.  

<u>Optie #3:</u> index entiteiten in een afzonderlijke partitie of een tabel maken  

Gebruik voor de derde optie, index-entiteiten die opslaan van de volgende gegevens:  

![][15]

De **EmployeeIDs** eigenschap bevat een lijst van de werknemer-id's voor werknemers met de achternaam opgeslagen in de **RowKey**.  

Met de derde optie, kunt u EGTs niet gebruiken om consistentie te behouden omdat de index-entiteiten in een afzonderlijke partitie van de werknemer entiteiten. U moet ervoor zorgen dat de index entiteiten uiteindelijk consistent is met de werknemer entiteiten.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Deze oplossing vereist ten minste twee query's naar het overeenkomende entiteiten ophalen: een query uitvoeren op de index-entiteiten voor de lijst van **RowKey** waarden en query's voor het ophalen van elke entiteit in de lijst.  
* Gezien het feit dat een afzonderlijke entiteit een maximale grootte van 1 MB heeft, optie #2 en de optie #3 in de oplossing wordt ervan uitgegaan dat de lijst met werknemer-id's voor een bepaalde achternaam nooit groter dan 1 MB is. Als de lijst met werknemer-id's kunnen niet groter zijn dan 1 MB groot is, gebruik van optie #1 en opslaan van de indexgegevens in de blob-opslag.  
* Als u de optie #2 gebruikt moet (met EGTs om af te handelen toevoegen en verwijderen van werknemers en het wijzigen van de achternaam van een werknemer) u nagaan of het volume van transacties worden de limieten voor schaalbaarheid in een bepaalde partitie benaderen. Als dit het geval is, moet u rekening houden met een uiteindelijk consistent oplossing (optie &#1; of optie #3) die gebruikmaakt van wachtrijen om de updateaanvragen te verwerken en kunt u uw index entiteiten opslaan op een afzonderlijke partitie van de werknemer entiteiten.  
* Optie #2 in deze oplossing wordt ervan uitgegaan dat u wilt zoeken op achternaam binnen een afdeling: bijvoorbeeld, u wilt ophalen van een lijst met werknemers met een achternaam Jones van de verkoopafdeling. Als u zoeken naar alle werknemers met een achternaam Jones over de hele organisatie wilt, optie #1 of optie #3 gebruiken.
* U kunt een oplossing op basis van wachtrijen die zorgt voor uiteindelijke consistentie implementeren (Zie de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) voor meer informatie).  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Gebruik dit patroon als u wilt voor het opzoeken van een set van entiteiten met algemene waarde van een eigenschap, zoals alle werknemers die over de achternaam Jones dezelfde.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  

### <a name="denormalization-pattern"></a>Denormalization patroon
Verwante gegevens samen combineren in één entiteit waarmee u kunt alle gegevens die u nodig hebt met een query één punt ophalen.  

#### <a name="context-and-problem"></a>Context en probleem
In een relationele database, moet u doorgaans gegevens als u wilt verwijderen, wat resulteert in query's die gegevens uit meerdere tabellen ophalen worden gedupliceerd normaliseren. Als u uw gegevens in Azure-tabellen normaliseren, moet u meerdere retouren van de client naar de server voor het ophalen van de bijbehorende gegevens. Bijvoorbeeld met de structuur van de tabel hieronder u moet twee retouren naar de details voor een afdeling ophalen: één voor het ophalen van de afdeling entiteit met id van de manager en vervolgens een andere aanvraag voor het ophalen van de manager details in een entiteit van de werknemer.  

![][16]

#### <a name="solution"></a>Oplossing
In plaats van de gegevens worden opgeslagen in twee afzonderlijke entiteiten, denormalize van de gegevens en een kopie van de details van de manager in de entiteit afdeling houden. Bijvoorbeeld:  

![][17]

Met de afdeling entiteiten met deze eigenschappen worden opgeslagen, kunt u nu alle gegevens die u moet over een afdeling in een query punt ophalen.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Er is enige kosten overhead die is gekoppeld aan twee keer sommige gegevens op te slaan. De prestatievoordelen (die voortvloeien uit minder aanvragen aan de storage-service) doorgaans belangrijker is dan de marginale toename van de kosten voor opslag (en deze kosten is gedeeltelijk gecompenseerd door een vermindering van het aantal transacties die u nodig hebt voor het ophalen van de details van een afdeling).  
* U moet de consistentie van de twee entiteiten die voor het opslaan van informatie over beheerders onderhouden. U kunt het probleem consistentiecontrole verwerkt met behulp van EGTs meerdere entiteiten in één transactie atomic bijwerken: in dit geval wordt de entiteit afdeling en de werknemer entiteit voor de afdelingsmanager worden opgeslagen in dezelfde partitie.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Dit patroon gebruikt als u regelmatig om verwante informatie te zoeken. Dit patroon vermindert het aantal query's die de client aanbrengen moet in de gegevens die vereist worden opgehaald.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)

### <a name="compound-key-pattern"></a>Samengestelde sleutel patroon
Gebruik samengestelde **RowKey** waarden om in te schakelen van een client voor het opzoeken van gerelateerde gegevens met een query één punt.  

#### <a name="context-and-problem"></a>Context en probleem
In een relationele database is het erg natuurlijke joins in query's gebruiken om te retourneren van gerelateerde delen van gegevens naar de client in één query op. Bijvoorbeeld, kunt u de werknemer-id te zoeken om een overzicht van gerelateerde entiteiten die prestaties bevatten en gegevens voor die werknemer bekijken.  

Stel dat u bij het opslaan van entiteiten van de werknemer in de tabel-service met behulp van de volgende structuur:  

![][18]

U moet er ook voor het opslaan van historische gegevens met betrekking tot beoordelingen en prestaties voor elk jaar die de werknemer heeft gewerkt voor uw organisatie en moet u toegang tot deze informatie per jaar. Een mogelijkheid is het maken van een andere tabel die wordt opgeslagen entiteiten met de volgende structuur:  

![][19]

U ziet dat met deze methode wilt u misschien dubbele bepaalde gegevens (zoals de voornaam en achternaam) in de nieuwe entiteit waarmee u uw gegevens met één aanvraag ophalen. U moet echter sterke consistentie kan niet onderhouden omdat u niet een EGT gebruiken voor het bijwerken van de twee entiteiten moment.  

#### <a name="solution"></a>Oplossing
Een nieuwe entiteitstype opslaan in de oorspronkelijke tabel met behulp van de entiteiten met de volgende structuur:  

![][20]

U ziet hoe de **RowKey** is nu een samengestelde sleutel bestaat uit de werknemer-id en het jaar van de evaluatie-gegevens waarmee u de prestaties van de werknemer ophalen en gegevens met één aanvraag voor een enkele entiteit te controleren.  

Het volgende voorbeeld ziet hoe u alle controle-gegevens kunt ophalen voor een bepaalde werknemer (zoals werknemer 000123 van de verkoopafdeling):  

$filter = (PartitionKey eq 'Sales') en (RowKey ge 'empid_000123') en (RowKey lt 'empid_000124') & $select = RowKey, beoordeling door Manager, beoordeling van de Peer, opmerkingen  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* U moet een geschikte scheidingsteken waarmee u gemakkelijk parseren gebruiken de **RowKey** waarde: bijvoorbeeld **000123_2012**.  
* U ook opslaat deze entiteit in dezelfde partitie als andere entiteiten die gerelateerde gegevens bevatten voor dezelfde werknemer, wat betekent dat u kunt EGTs sterke consistentie.
* U moet overwegen hoe vaak u een query uit op de gegevens om te bepalen of dit patroon geschikt is.  Bijvoorbeeld, als u toegang hebben tot de gegevens van revisie zelden en vaak de belangrijkste werknemersgegevens bewaar ze als afzonderlijke entiteiten.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Gebruik dit patroon wanneer u wilt opslaan op een of meer entiteiten die query u vaak gerelateerde.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)  
* [Werken met heterogene Entiteitstypen](#working-with-heterogeneous-entity-types)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  

### <a name="log-tail-pattern"></a>Patroon voor logboekbestand staart
Ophalen van de  *n*  entiteiten die onlangs zijn toegevoegd aan een partitie met behulp van een **RowKey** waarde die in omgekeerde datum en tijd volgorde worden gesorteerd.  

#### <a name="context-and-problem"></a>Context en probleem
Een algemene vereiste is mogelijk het meest recent gemaakte entiteiten ophalen, bijvoorbeeld de meest recente tien claims die zijn ingediend door een werknemer onkosten. Tabel ondersteuning vraagt een **$top** querybewerking te retourneren van de eerste  *n*  entiteiten uit een set: Er is geen equivalent querybewerking te retourneren van de laatste n entiteiten in een set.  

#### <a name="solution"></a>Oplossing
Opslaan van de entiteiten met een **RowKey** dat natuurlijk sorteren in volgorde van de omgekeerde datum/tijd met behulp van zodat de meest recente vermelding is altijd de eerste rij in de tabel.  

Bijvoorbeeld, om te kunnen ophalen van de tien meest recente onkosten claims ingediend door een werknemer, kunt u een omgekeerde maatstreepjes-waarde die is afgeleid van de huidige datum en tijd. De volgende C#-codevoorbeeld ziet u een manier om u te maken van een geschikte waarde 'omgekeerde ticks' voor een **RowKey** die sorteren van de meest recente naar oud:  

`string invertedTicks = string.Format("{0:D19}", DateTime.MaxValue.Ticks - DateTime.UtcNow.Ticks);`  

U kunt teruggaan naar de datum-tijdwaarde met de volgende code:  

`DateTime dt = new DateTime(DateTime.MaxValue.Ticks - Int64.Parse(invertedTicks));`  

De tabelquery ziet er als volgt:  

`https://myaccount.table.core.windows.net/EmployeeExpense(PartitionKey='empid')?$top=10`  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* U moet de waarde van de omgekeerde maatstreepjes met nullen om te controleren of dat de tekenreekswaarde sorteert zoals verwacht toonaangevende opgevuld.  
* U moet rekening houden met de schaalbaarheidsdoelen op het niveau van een partitie. Wees voorzichtig hotspot partities niet maken.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Dit patroon gebruikt als u toegang tot entiteiten in volgorde van de omgekeerde datum/tijd- of wanneer u toegang wilt tot de meest recent toegevoegde entiteiten.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Toevoegen / antivirusprogramma patroon toevoegen](#prepend-append-anti-pattern)  
* [Entiteiten ophalen](#retrieving-entities)  

### <a name="high-volume-delete-pattern"></a>Patroon voor grote volumes verwijderen
Het verwijderen van een groot aantal entiteiten inschakelen door het opslaan van alle entiteiten voor gelijktijdige verwijdering in hun eigen afzonderlijke tabel. u kunt de entiteiten verwijderen door de tabel verwijderen.  

#### <a name="context-and-problem"></a>Context en probleem
Veel toepassingen verwijderen oude gegevens die niet langer beschikbaar zijn voor een clienttoepassing of de toepassing naar een ander opslagmedium is gearchiveerd. U doorgaans dergelijke gegevens identificeren met een datum: bijvoorbeeld: u hebt een vereiste om records van alle aanmeldingsaanvragen voor die meer dan 60 dagen te verwijderen.  

Een mogelijke ontwerp is het gebruik van de datum en tijd van de aanmeldingsaanvraag in de **RowKey**:  

![][21]

Deze aanpak voorkomt partitie hotspots omdat de toepassing kunt invoegen en verwijderen van aanmelding entiteiten voor elke gebruiker in een afzonderlijke partitie. Deze aanpak kan echter kostbaar en tijd in beslag nemen als u een groot aantal entiteiten hebt omdat u eerst een tabelscan uitvoeren om te kunnen identificeren van de entiteiten moet te verwijderen en vervolgens moet u elke oude entiteit verwijderen zijn. Houd er rekening mee dat u het aantal retouren naar de server vereist voor het verwijderen van de oude entiteiten met meerdere delete-aanvragen in EGTs batchverwerking kunt verkleinen.  

#### <a name="solution"></a>Oplossing
Een afzonderlijke tabel gebruiken voor elke dag van aanmeldingspogingen. U kunt het ontwerp van de entiteit bovenstaande hotspots voorkomen wanneer entiteiten invoegen en verwijderen van oude entiteiten nu gewoon een vraag is van het verwijderen van één tabel elke dag (een enkele opslagbewerking) in plaats van zoeken en verwijderen van honderden en duizenden afzonderlijke aanmelding entiteiten elke dag.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Uw ontwerp biedt ondersteuning voor andere manieren de gegevens zoals het opzoeken van specifieke entiteiten, koppelen aan andere gegevens of genereren verzamelde gegevens zullen worden gebruikt door uw toepassing?  
* Uw ontwerp hotspots voorkomen tijdens het invoegen van nieuwe entiteiten  
* Een vertraging verwachten als u gebruiken als de naam van de dezelfde tabel wilt na het verwijderen. Het is beter gebruik altijd uniek tabelnamen.  
* Verwachten dat sommige beperking wanneer u eerst een nieuwe tabel terwijl de tabelservice de toegangspatronen leert en distributie van de partities over knooppunten. U moet rekening houden hoe vaak moet u nieuwe tabellen maken.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Dit patroon gebruiken wanneer u een groot aantal entiteiten die op hetzelfde moment moet u verwijderen.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)
* [Entiteiten wijzigen](#modifying-entities)  

### <a name="data-series-pattern"></a>Patroon van de reeks gegevens
Store voltooid gegevensreeksen in één entiteit om te beperken het aantal aanvragen die u aanbrengt.  

#### <a name="context-and-problem"></a>Context en probleem
Er is een veelvoorkomend scenario voor een toepassing voor het opslaan van een reeks met gegevens die normaal gesproken nodig is om op te halen in één keer. Uw toepassing kan bijvoorbeeld opname hoeveel IM-berichten elke werknemer elk uur en vervolgens deze informatie gebruiken om te tekenen hoeveel berichten elke gebruiker die is verzonden via de voorgaande 24 uur. Een ontwerp kan voor het opslaan van 24 entiteiten voor elke werknemer zijn:  

![][22]

Bij dit ontwerp kunt u eenvoudig zoeken en bijwerken van de entiteit voor elke werknemer bijwerken wanneer de toepassing moet de waarde voor aantal bijwerken. Voor het ophalen van de informatie voor het tekenen van een grafiek van de activiteit voor de voorgaande 24 uur, moet u echter 24 entiteiten ophalen.  

#### <a name="solution"></a>Oplossing
Het ontwerp van de volgende voor het opslaan van het aantal berichten voor elk uur een afzonderlijke eigenschap gebruiken:  

![][23]

Bij dit ontwerp kunt u een samenvoegbewerking voor het aantal berichten voor een werknemer bijwerken voor een specifiek uur. U kunt nu alle informatie die u nodig hebt om het gebruik van een aanvraag voor een enkele entiteit diagram te tekenen ophalen.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Als de volledige reeks past niet in één entiteit (een entiteit kan maximaal 252 eigenschappen hebben), gebruikt u een alternatieve gegevensarchief, zoals een blob.  
* Als u meerdere clients tegelijkertijd bijwerken van een entiteit hebt, moet u gebruik van de **ETag** optimistische gelijktijdigheid implementeren. Als u veel clients hebt, kunt u hoge conflicten ondervinden.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Dit patroon gebruiken als u wilt bijwerken en ophalen van een reeks die is gekoppeld aan een afzonderlijke entiteit.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Grote entiteiten patroon](#large-entities-pattern)  
* [Samenvoegen of vervangen](#merge-or-replace)  
* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) (als u de gegevensreeks in een blob opslaat)  

### <a name="wide-entities-pattern"></a>Wide entiteiten patroon
Gebruik van meerdere fysieke entiteiten voor het opslaan van logische entiteiten met meer dan 252 eigenschappen.  

#### <a name="context-and-problem"></a>Context en probleem
Een afzonderlijke entiteit kan maximaal 252 eigenschappen (met uitzondering van de verplichte eigenschappen) en kan niet meer dan 1 MB aan gegevens opslaan in totaal. In een relationele database, doorgaans krijgt u ronde beperkingen met betrekking tot de grootte van een rij bij het toevoegen van een nieuwe tabel en een 1-op-1-relatie tussen deze twee afdwingen.  

#### <a name="solution"></a>Oplossing
De tabel-service gebruikt, kunt u meerdere entiteiten ter vertegenwoordiging van een object één grote bedrijven met meer dan 252 eigenschappen opslaan. Bijvoorbeeld, als u een telling van het aantal IM-berichten is verzonden door elke werknemer voor de afgelopen 365 dagen opslaan wilt, kunt u het ontwerp van de volgende die gebruikmaakt van twee entiteiten met verschillende schema's:  

![][24]

U kunt een EGT gebruiken als u een wijziging aanbrengt die is vereist voor het bijwerken van beide entiteiten wilt zodat ze zijn gesynchroniseerd met elkaar. Anders kunt u één samenvoegbewerking bijwerken van het aantal berichten voor een specifieke dag. Alle gegevens ophalen voor een afzonderlijke werknemer moet u beide entiteiten, kunt u doen met twee efficiënte aanvragen die gebruikmaken van beide ophalen een **PartitionKey** en een **RowKey** waarde.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Bij het ophalen van een volledige logische entiteit ten minste twee opslagtransacties omvat: een voor elke fysieke entiteit ophalen.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Gebruik dit patroon wanneer nodig voor het opslaan van entiteiten waarvan de grootte van of het aantal eigenschappen de grenzen voor een afzonderlijke entiteit in de tabel-service overschrijdt.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Entiteit groep transacties](#entity-group-transactions)
* [Samenvoegen of vervangen](#merge-or-replace)

### <a name="large-entities-pattern"></a>Grote entiteiten patroon
Blob storage gebruiken voor het opslaan van grote eigenschapswaarden.  

#### <a name="context-and-problem"></a>Context en probleem
Een afzonderlijke entiteit kan niet meer dan 1 MB aan gegevens opslaan in totaal. Als een of meer van de eigenschappen van uw waarden die ertoe leiden dat de totale grootte van uw entiteit zijn dan deze waarde kunt opslaan, kunt u het geheel niet opslaan in de tabel-service.  

#### <a name="solution"></a>Oplossing
Als uw entiteit 1 MB groot overschrijdt omdat een of meer eigenschappen een grote hoeveelheid gegevens bevatten, kunt u gegevens opslaan in de Blob-service en het adres van de blob op te slaan in een eigenschap in de entiteit. Bijvoorbeeld, u kunt de foto van een werknemer opslaan in blob-opslag en opslaan van een koppeling naar de foto in de **Photo** eigenschap van de werknemer entiteit:  

![][25]

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Gebruiken om te blijven uiteindelijke consistentie tussen de entiteit in de tabel-service en de gegevens in de Blob-service, de [uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern) uw entiteiten onderhouden.
* Bij het ophalen van een volledige entiteit ten minste twee opslagtransacties omvat: één voor het ophalen van de entiteit en één voor het ophalen van de blob-gegevens.  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Dit patroon gebruikt als u voor het opslaan van entiteiten waarvan de grootte de grenzen voor een afzonderlijke entiteit in de tabel-service overschrijdt.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Uiteindelijk consistent transacties patroon](#eventually-consistent-transactions-pattern)  
* [Wide entiteiten patroon](#wide-entities-pattern)

<a name="prepend-append-anti-pattern"></a>

### <a name="prependappend-anti-pattern"></a>Antivirusprogramma patroon toevoegen/toevoegen
Schaalbaarheid vergroten wanneer u een groot aantal invoegingen hebt met de invoegt verspreid over meerdere partities.  

#### <a name="context-and-problem"></a>Context en probleem
Voorafgaand of entiteiten doorgaans in te voegen aan uw opgeslagen entiteiten resulteert in de toepassing nieuwe entiteiten toe te voegen aan de eerste of laatste partitie van een reeks van partities. In dit geval plaatsvinden alle van de invoegt op elk moment in dezelfde partitie maken van een hotspot waarmee wordt voorkomen de tabelservice van load voegt te verdelen over meerdere knooppunten en mogelijk veroorzaakt door uw toepassing dat om de schaalbaarheidsdoelen voor partitie. Bijvoorbeeld, als u een toepassing hebt die netwerk logboeken en toegang tot bedrijfsbronnen door werknemers, klikt u vervolgens een entiteit structuur zoals hieronder wordt weergegeven in het huidige uur partitie een hotspot is als het volume van transacties het doel van de schaalbaarheid van een afzonderlijke partitie bereikt kan leiden:  

![][26]

#### <a name="solution"></a>Oplossing
De volgende alternatieve entiteit structuur voorkomt een hotspot op een bepaalde partitie als de toepassing Logboeken gebeurtenissen:  

![][27]

Kennisgeving met dit voorbeeld van hoe beide de **PartitionKey** en **RowKey** samengestelde sleutels. De **PartitionKey** gebruikt de afdeling en de werknemer-id om de logboekregistratie verdelen over meerdere partities.  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten bij het bepalen van het implementeren van dit patroon:  

* Ondersteunt de alternatieve sleutelstructuur die voorkomt efficiënt hot partities maken op voegt de query's kunt u de clienttoepassing?  
* Het verwachte volume van transacties betekent dat u waarschijnlijk de schaalbaarheidsdoelen voor een afzonderlijke partitie bereiken en worden beperkt door de storage-service?  

#### <a name="when-to-use-this-pattern"></a>Het gebruik van dit patroon
Vermijd het prepend/append anti-patroon wanneer het volume van transacties kunnen ertoe leiden dat door de storage-service beperken wanneer u een hot partitie is.  

#### <a name="related-patterns-and-guidance"></a>Verwante patronen en richtlijnen
De volgende patronen en richtlijnen mogelijk ook relevante bij het implementeren van dit patroon:  

* [Samengestelde sleutel patroon](#compound-key-pattern)  
* [Patroon voor logboekbestand staart](#log-tail-pattern)  
* [Entiteiten wijzigen](#modifying-entities)  

### <a name="log-data-anti-pattern"></a>Antivirusprogramma patroon voor logboekbestand gegevens
Normaal gesproken moet u de Blob-service in plaats van de tabel-service voor het opslaan van gegevens aan het logboek.  

#### <a name="context-and-problem"></a>Context en probleem
Als u een algemeen gebruiksvoorbeeld voor logboekgegevens is voor het ophalen van een selectie van vermeldingen in het logboek voor een specifieke datum/tijd-bereik: bijvoorbeeld wordt gezocht naar alle fout en kritieke berichten die uw toepassing vastgelegd tussen 15:04 en 15:06 op een bepaalde datum. U niet wilt dat de datum en tijd van het logboekbericht gebruiken om te bepalen van de partitie opslaan van entiteiten logboek: die resulteert in een hot partitie omdat er op elk gewenst alle entiteiten in het logboek wordt delen dezelfde **PartitionKey** waarde (Zie de sectie [antivirusprogramma patroon Prepend/append](#prepend-append-anti-pattern)). Het volgende schema van de entiteit voor een logboekbericht resulteert bijvoorbeeld in een hot partitie omdat de toepassing alle berichten in het logboek voor de partitie voor de huidige datum en het uur schrijft:  

![][28]

In dit voorbeeld wordt de **RowKey** bevat de datum en tijd van het logboekbericht om ervoor te zorgen dat logboekberichten worden opgeslagen in datum/tijd-volgorde gesorteerd en bevat een bericht-id als meerdere logboekberichten de dezelfde datum en tijd delen.  

Een andere manier is het gebruik van een **PartitionKey** die ervoor zorgt dat de toepassing berichten over een reeks partities schrijft. Als de bron van het logboekbericht een manier biedt voor de distributie van berichten over veel partities, kan u bijvoorbeeld het volgende schema voor de entiteit gebruiken:  

![][29]

Het probleem met dit schema is echter voor het ophalen van de logboekberichten voor een bepaalde periode moet u elke partitie zoeken in de tabel.

#### <a name="solution"></a>Oplossing
De vorige sectie het probleem van de tabel-service gebruiken voor het opslaan van logboekvermeldingen en voorgestelde twee onvoldoende, ontwerpen probeert gemarkeerd. Een oplossing heeft geleid tot een hot partitie met het risico van slechte prestaties schrijven logboekberichten; de andere oplossing heeft geresulteerd in slechte queryprestaties vanwege de vereisten voor het scannen van elke partitie in de tabel voor het ophalen van berichten in het logboek voor een bepaalde periode. BLOB storage biedt een betere oplossing voor dit soort scenario en dit is hoe Azure Storage Analytics slaat de logboekgegevens worden verzameld.  

Deze sectie geeft een overzicht van hoe opslag Analytics logboekgegevens opslaat in de blob-opslag ter illustratie van deze benadering voor het opslaan van gegevens die u doorgaans een query voor het bereik.  

Storage Analytics slaat logboekberichten in een indeling met scheidingstekens in meerdere blobs. De indeling gescheiden eenvoudig een clienttoepassing parseren van de gegevens in het logboekbericht.  

Storage Analytics maakt gebruik van een naamgevingsconventie voor blobs die kunt u de blob vinden (of BLOB's) die de berichten in het logboek waarnaar u zoekt bevatten. Een blob met de naam 'queue/2014/07/31/1800/000001.log' bevat bijvoorbeeld logboekberichten die betrekking hebben op de queue-service voor het uur om 18:00 uur op 31 juli 2014 wordt gestart. De '000001' geeft aan dat dit het eerste logboekbestand voor deze periode. Storage Analytics registreert ook de tijdstempel van de eerste en laatste logboekberichten opgeslagen in het bestand als onderdeel van de blob-metagegevens. De API voor blob-opslag kunt u blobs niet in een container op basis van een voorvoegsel vinden: om te zoeken in alle blobs die wachtrij logboekgegevens bevatten voor het beginnen bij 18:00 uur, kunt u het voorvoegsel "wachtrij/2014/07/31/1800."  

Storage Analytics buffers Meld berichten intern en vervolgens regelmatig updates van de juiste blob of maakt u een nieuw bestand met de meest recente batch van logboekvermeldingen. Dit vermindert het aantal schrijft die deze naar de blob-service moet uitvoeren.  

Als u een vergelijkbare oplossing in uw eigen toepassing implementeert, moet u rekening houden met het beheren van de verhouding tussen betrouwbaarheid (elke logboekvermelding schrijven naar de blob-opslag als dit gebeurt) en de kosten en schaalbaarheid (buffer updates in uw toepassing en worden geschreven naar blob storage in batches).  

#### <a name="issues-and-considerations"></a>Problemen en overwegingen
Houd rekening met de volgende punten wanneer u beslist het opslaan van gegevens aan het logboek:  

* Als u een tabelontwerp dat potentiële hot partities voorkomt maakt, merkt u dat u geen toegang uw logboekgegevens efficiënt tot.  
* Voor het verwerken van gegevens aan het logboek moet een client vaak veel records laden.  
* Hoewel logboekgegevens is vaak opgebouwd, kan de blob-opslag een betere oplossing zijn.  

### <a name="implementation-considerations"></a>Overwegingen bij de implementatie
Deze sectie worden enkele van de overwegingen op moet letten wanneer u de patronen die zijn beschreven in de vorige secties implementeert beschreven. De meeste van deze sectie bevat voorbeelden geschreven in C# en die gebruikmaken van de Storage-clientbibliotheek (versie 4.3.0 op het moment van schrijven).  

### <a name="retrieving-entities"></a>Entiteiten ophalen
Zoals beschreven in de sectie [ontwerp voor het uitvoeren van query's](#design-for-querying), de meest efficiënte query is een punt-query. In sommige scenario's moet u mogelijk echter meerdere entiteiten ophalen. Deze sectie beschrijft een aantal algemene benaderingen bij het ophalen van entiteiten met behulp van de Storage-clientbibliotheek.  

#### <a name="executing-a-point-query-using-the-storage-client-library"></a>Uitvoeren van een punt-query met behulp van de Storage-clientbibliotheek
De eenvoudigste manier om het uitvoeren van een punt-query is met de **ophalen** bewerking tabel, zoals wordt weergegeven in het volgende C# codefragment die ophaalt van een entity met een **PartitionKey** van de waarde 'Verkoop' en een **RowKey** van de waarde '212':  

```csharp
TableOperation retrieveOperation = TableOperation.Retrieve<EmployeeEntity>("Sales", "212");
var retrieveResult = employeeTable.Execute(retrieveOperation);
if (retrieveResult.Result != null)
{
    EmployeeEntity employee = (EmployeeEntity)retrieveResult.Result;
    ...
}  
```

U ziet hoe de entiteit in dit voorbeeld worden verwacht opgehaald om te worden van het type **EmployeeEntity**.  

#### <a name="retrieving-multiple-entities-using-linq"></a>Bij het ophalen van meerdere entiteiten met behulp van LINQ
U kunt meerdere entiteiten ophalen met behulp van LINQ met Storage-clientbibliotheek en het opgeven van een query met een **waar** component. Om te voorkomen dat een tabelscan, moet u altijd opnemen de **PartitionKey** waarde in de where-component, en indien mogelijk de **RowKey** waarde om te voorkomen dat de tabel en partitie scans. De tabelservice ondersteunt een beperkte set vergelijkingsoperators (groter dan groter dan of gelijk is, minder dan, kleiner dan of gelijk zijn, gelijk en niet gelijk) te gebruiken in de where component. De volgende C#-codefragment vindt alle werknemers waarvan de laatste naam begint met "B" (ervan uitgaande dat de **RowKey** slaat de achternaam) in de afdeling verkoop (ervan uitgaande dat de **PartitionKey** slaat de afdelingsnaam):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = employeeTable.CreateQuery<EmployeeEntity>();
var query = (from employee in employeeQuery
            where employee.PartitionKey == "Sales" &&
            employee.RowKey.CompareTo("B") >= 0 &&
            employee.RowKey.CompareTo("C") < 0
            select employee).AsTableQuery();
var employees = query.Execute();  
```

U ziet hoe de query is zowel een **RowKey** en een **PartitionKey** betere prestaties te garanderen.  

Het volgende codevoorbeeld toont dezelfde functionaliteit beheersen API gebruiken (Zie voor meer informatie over beheersen API's in het algemeen [aanbevolen procedures voor het ontwerpen van een beheersen API](http://visualstudiomagazine.com/articles/2013/12/01/best-practices-for-designing-a-fluent-api.aspx)):  

```csharp
TableQuery<EmployeeEntity> employeeQuery = new TableQuery<EmployeeEntity>().Where(
    TableQuery.CombineFilters(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition(
    "PartitionKey", QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.GenerateFilterCondition(
    "RowKey", QueryComparisons.GreaterThanOrEqual, "B")
),
TableOperators.And,
TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "C")
    )
);
var employees = employeeTable.ExecuteQuery(employeeQuery);  
```

> [!NOTE]
> Het voorbeeld worden genest meerdere **CombineFilters** methoden voor het opnemen van de drie filtervoorwaarden.  
> 
> 

#### <a name="retrieving-large-numbers-of-entities-from-a-query"></a>Groot aantal entiteiten ophalen uit een query
Een optimale query retourneert een afzonderlijke entiteit op basis van een **PartitionKey** waarde en een **RowKey** waarde. In sommige scenario's hebben u echter een vereiste is te groot aantal entiteiten retourneren vanuit dezelfde partitie of zelfs via veel partities.  

In dergelijke gevallen moet u altijd volledig de prestaties van uw toepassing testen.  

Een query op de tabelservice kan maximaal 1000 entiteiten in één keer worden geretourneerd en wordt uitgevoerd voor een maximum van vijf seconden. Als de resultatenset bevat meer dan 1000 entiteiten, als de query is niet voltooid binnen vijf seconden, of als de query overschrijdt de grens van de partitie is, retourneert de tabel-service een vervolgtoken zodat de clienttoepassing om aan te vragen van de volgende set van entiteiten. Zie voor meer informatie over hoe voortzetting werk tokens [querytime-out en paginering](http://msdn.microsoft.com/library/azure/dd135718.aspx).  

Als u de Storage-clientbibliotheek gebruikt, kan deze automatisch voortzetting tokens voor u verwerken als deze entiteiten uit de tabelservice retourneert. De volgende C# voorbeeldcode met behulp van de Storage-clientbibliotheek automatisch verwerkt voortzetting tokens als de tabelservice die in een antwoord retourneert:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

var employees = employeeTable.ExecuteQuery(employeeQuery);
foreach (var emp in employees)
{
        ...
}  
```

De volgende C#-code voortzetting tokens expliciet worden verwerkt:  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

TableContinuationToken continuationToken = null;

do
{
        var employees = employeeTable.ExecuteQuerySegmented(
        employeeQuery, continuationToken);
    foreach (var emp in employees)
    {
    ...
    }
    continuationToken = employees.ContinuationToken;
} while (continuationToken != null);  
```

Voortzetting tokens expliciet gebruikt, kunt u bepalen wanneer de toepassing wordt het volgende segment van gegevens wordt opgehaald. Bijvoorbeeld, als uw clienttoepassing kan gebruikers op de pagina via de entiteiten die zijn opgeslagen in een tabel, een gebruiker kan niet meer wilt pagina via de entiteiten die zijn opgehaald door de query zodat uw toepassing een vervolgtoken alleen gebruiken zou voor het ophalen van het volgende segment wanneer de gebruiker had voltooid paging via alle entiteiten in het huidige segment. Deze methode biedt verschillende voordelen:  

* Hiermee kunt u de hoeveelheid gegevens op te halen uit de tabel-service beperken en die u via het netwerk verplaatst.  
* Hiermee kunt u het uitvoeren van asynchrone IO in .NET.  
* Hiermee kunt u voor het serialiseren van het vervolgtoken naar de permanente opslag zodat u kunt doorgaan in het geval van een toepassing is vastgelopen.  

> [!NOTE]
> Een vervolgtoken retourneert doorgaans een segment met 1000 entiteiten, hoewel deze mogelijk minder. Dit is ook het geval als u het aantal vermeldingen met behulp van een query retourneert beperken **nemen** te retourneren van de eerste n entiteiten die voldoen aan uw criteria lookup: de tabelservice kan een segment met minder dan n entiteiten samen met een vervolgtoken waarmee u kunt de resterende entiteiten ophalen geretourneerd.  
> 
> 

De volgende C#-code wordt getoond hoe u het aantal entiteiten die zijn geretourneerd binnen een segment wijzigen:  

```csharp
employeeQuery.TakeCount = 50;  
```

#### <a name="server-side-projection"></a>Projectie-serverzijde
Één entiteit kan maximaal 255 eigenschappen hebben en maximaal 1 MB groot zijn. Wanneer u een query uitvoeren op de tabel en entiteiten ophalen, moet u wellicht niet alle eigenschappen en ervaren onnodig (als u wilt verminderen en de kosten van latentie) kunt voorkomen. U kunt serverzijde projectie gebruiken om over te dragen alleen de eigenschappen die u nodig hebt. Het volgende voorbeeld is haalt alleen de **e** eigenschap (samen met **PartitionKey**, **RowKey**, **tijdstempel**, en **ETag**) van de entiteiten die zijn geselecteerd door de query.  

```csharp
string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
List<string> columns = new List<string>() { "Email" };
TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter).Select(columns);

var entities = employeeTable.ExecuteQuery(employeeQuery);
foreach (var e in entities)
{
        Console.WriteLine("RowKey: {0}, EmployeeEmail: {1}", e.RowKey, e.Email);
}  
```

U ziet hoe de **RowKey** waarde is beschikbaar, zelfs als deze niet is opgenomen in de lijst met eigenschappen om op te halen.  

### <a name="modifying-entities"></a>Entiteiten wijzigen
De Storage-clientbibliotheek kunt u de entiteiten die zijn opgeslagen in de tabelservice door invoegen, verwijderen en bijwerken van entiteiten wijzigen. U kunt EGTs batch meerdere insert, update en delete-bewerkingen samen in Verminder het aantal retouren vereist en de prestaties van uw oplossing verbeteren.  

Rekening mee dat uitzonderingen die worden gegenereerd wanneer de Opslagclientbibliotheek wordt uitgevoerd een EGT doorgaans de index van de entiteit die de batch mislukken veroorzaakt. Dit is handig wanneer u code die gebruikmaakt van EGTs foutopsporing.  

U moet ook overwegen hoe uw ontwerp is van invloed op hoe de clienttoepassing gelijktijdigheid van taken en updatebewerkingen verwerkt.  

#### <a name="managing-concurrency"></a>Gelijktijdigheid van taken beheren
Standaard implementeert de tabelservice optimistische gelijktijdigheid controleert op het niveau van afzonderlijke entiteiten voor **invoegen**, **samenvoegen**, en **verwijderen** bewerkingen, hoewel het mogelijk voor een client om af te dwingen van de tabelservice voor het overslaan van deze controles. Zie voor meer informatie over hoe de tabelservice gelijktijdigheid beheert [gelijktijdigheid beheren in Microsoft Azure Storage](../storage/common/storage-concurrency.md).  

#### <a name="merge-or-replace"></a>Samenvoegen of vervangen
De **vervangen** methode van de **TableOperation** klasse altijd vervangen door de volledige entiteit in de tabel-service. Als u geen een eigenschap in de aanvraag wanneer deze eigenschap in de opgeslagen entiteit bestaat, wordt in de aanvraag die eigenschap verwijdert uit de opgeslagen entiteit. Tenzij u een eigenschap expliciet verwijderen uit een opgeslagen entiteit wilt, moet u elke eigenschap opnemen in de aanvraag.  

U kunt de **samenvoegen** methode van de **TableOperation** klasse om te verminderen de hoeveelheid gegevens die u naar de tabel-service verzendt wanneer u wilt bijwerken van een entiteit. De **samenvoegen** methode alle eigenschappen in de opgeslagen entiteit vervangen door waarden van eigenschappen van de entiteit die is opgenomen in de aanvraag, maar blijft behouden alle eigenschappen in de opgeslagen entiteit die niet zijn opgenomen in de aanvraag. Dit is handig als u grote entiteiten en hoeft slechts een klein aantal eigenschappen in een aanvraag bijwerken.  

> [!NOTE]
> De **vervangen** en **samenvoegen** twee methoden mislukken als de entiteit niet bestaat. Als alternatief kunt u de **InsertOrReplace** en **InsertOrMerge** methoden die nieuwe entiteit maken als deze niet bestaat.  
> 
> 

### <a name="working-with-heterogeneous-entity-types"></a>Werken met heterogene Entiteitstypen
De tabel-service is een *schema minder* tabel archief dat betekent dat één tabel entiteiten van meerdere typen bieden geweldige flexibiliteit in uw ontwerp kunt opslaan. Het volgende voorbeeld wordt een tabel die het opslaan van zowel werknemer en entiteiten van de afdeling:  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Naam van de afdeling</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

Denk eraan dat elke entiteit moet nog steeds **PartitionKey**, **RowKey**, en **tijdstempel** waarden, maar mogelijk elke gewenste set eigenschappen. Bovendien, er is niets om aan te geven van het type van een entiteit, tenzij u ervoor kiest die gegevens ergens op te slaan. Er zijn twee opties voor het identificeren van het entiteitstype:  

* Toevoegen van het entiteitstype voor de **RowKey** (of mogelijk de **PartitionKey**). Bijvoorbeeld: **EMPLOYEE_000123** of **DEPARTMENT_SALES** als **RowKey** waarden.  
* Gebruik een afzonderlijke eigenschap voor het vastleggen van het entiteitstype, zoals wordt weergegeven in de onderstaande tabel.  

<table>
<tr>
<th>PartitionKey</th>
<th>RowKey</th>
<th>Timestamp</th>
<th></th>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td>Werknemer</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td>Werknemer</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Naam van de afdeling</th>
<th>EmployeeCount</th>
</tr>
<tr>
<td>Afdeling</td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
<td>
<table>
<tr>
<th>EntityType</th>
<th>Voornaam</th>
<th>Achternaam</th>
<th>Leeftijd</th>
<th>E-mail</th>
</tr>
<tr>
<td>Werknemer</td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</table>
</td>
</tr>
</table>

De eerste optie, de entiteit prepending type de **RowKey**, is handig als er een kans bestaat dat twee entiteiten met verschillende typen wellicht dezelfde sleutelwaarde. Deze groepen ook entiteiten van hetzelfde type samen in de partitie.  

De technieken beschreven in deze sectie zijn vooral relevant zijn voor de bespreking van de [relaties voor overname](#inheritance-relationships) eerder in deze handleiding in de sectie [modellering relaties](#modelling-relationships).  

> [!NOTE]
> U moet rekening houden met inbegrip van een uniek versienummer op in de waarde van het type entiteit zodat clienttoepassingen ontwikkelen POCO-objecten en werken met verschillende versies.  
> 
> 

De rest van deze sectie beschrijft een aantal van de functies in de Storage-clientbibliotheek die werken met meerdere Entiteitstypen in dezelfde tabel vergemakkelijken.  

#### <a name="retrieving-heterogeneous-entity-types"></a>Heterogene Entiteitstypen ophalen
Als u van de Storage-clientbibliotheek gebruikmaakt, hebt u drie opties voor het werken met meerdere Entiteitstypen.  

Als u welk type van de entiteit die is opgeslagen met een specifieke weet **RowKey** en **PartitionKey** waarden, daarna u het entiteitstype opgeven kunt wanneer u de entiteit ophalen, zoals wordt weergegeven in de vorige twee voorbeelden die entiteiten van het type ophalen **EmployeeEntity**: [uitvoeren van een punt-query met behulp van de Storage-clientbibliotheek](#executing-a-point-query-using-the-storage-client-library) en [bij het ophalen van meerdere entiteiten met behulp van LINQ](#retrieving-multiple-entities-using-linq).  

De tweede optie is met de **DynamicTableEntity** type (een eigenschappenverzameling) in plaats van een concreet POCO entiteitstype (deze optie ook de prestaties mogelijk verbeterd omdat het is niet nodig voor het serialiseren en deserialiseren van de entiteit .NET-typen). De volgende C#-code mogelijk meerdere entiteiten met verschillende typen opgehaald uit de tabel, maar retourneert alle entiteiten als **DynamicTableEntity** exemplaren. Vervolgens wordt de **EntityType** eigenschap om te bepalen van het type van elke entiteit:  

```csharp
string filter = TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("PartitionKey",
    QueryComparisons.Equal, "Sales"),
    TableOperators.And,
    TableQuery.CombineFilters(
    TableQuery.GenerateFilterCondition("RowKey",
                    QueryComparisons.GreaterThanOrEqual, "B"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey",
        QueryComparisons.LessThan, "F")
    )
);
TableQuery<DynamicTableEntity> entityQuery =
    new TableQuery<DynamicTableEntity>().Where(filter);
var employees = employeeTable.ExecuteQuery(entityQuery);

IEnumerable<DynamicTableEntity> entities = employeeTable.ExecuteQuery(entityQuery);
foreach (var e in entities)
{
EntityProperty entityTypeProperty;
if (e.Properties.TryGetValue("EntityType", out entityTypeProperty))
{
    if (entityTypeProperty.StringValue == "Employee")
    {
        // Use entityTypeProperty, RowKey, PartitionKey, Etag, and Timestamp
        }
    }
}  
```

Houd er rekening mee dat voor het ophalen van andere eigenschappen moet u de **TryGetValue** methode op de **eigenschappen** eigenschap van de **DynamicTableEntity** klasse.  

Een derde optie is om te combineren met behulp van de **DynamicTableEntity** type en een **EntityResolver** exemplaar. Hiermee kunt u omzetten in meerdere POCO-typen in dezelfde query. In dit voorbeeld wordt de **EntityResolver** gemachtigde maakt gebruik van de **EntityType** eigenschap onderscheid maken tussen de twee typen entiteit die de query retourneert. De **los** methode gebruikt de **resolver** gemachtigde om op te lossen **DynamicTableEntity** exemplaren te **TableEntity** exemplaren.  

```csharp
EntityResolver<TableEntity> resolver = (pk, rk, ts, props, etag) =>
{

        TableEntity resolvedEntity = null;
        if (props["EntityType"].StringValue == "Department")
        {
        resolvedEntity = new DepartmentEntity();
        }
        else if (props["EntityType"].StringValue == "Employee")
        {
        resolvedEntity = new EmployeeEntity();
        }
        else throw new ArgumentException("Unrecognized entity", "props");

        resolvedEntity.PartitionKey = pk;
        resolvedEntity.RowKey = rk;
        resolvedEntity.Timestamp = ts;
        resolvedEntity.ETag = etag;
        resolvedEntity.ReadEntity(props, null);
        return resolvedEntity;
};

string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, "Sales");
TableQuery<DynamicTableEntity> entityQuery =
        new TableQuery<DynamicTableEntity>().Where(filter);

var entities = employeeTable.ExecuteQuery(entityQuery, resolver);
foreach (var e in entities)
{
        if (e is DepartmentEntity)
        {
    ...
        }
        if (e is EmployeeEntity)
        {
    ...
        }
}  
```

#### <a name="modifying-heterogeneous-entity-types"></a>Heterogene Entiteitstypen wijzigen
U hoeft niet te weten van het type van een entiteit wilt verwijderen en u het type van een entiteit altijd weet wanneer u het invoegen. U kunt echter **DynamicTableEntity** type zonder te weten van het type en zonder gebruik van een POCO-entiteitsklasse bijwerken van een entiteit. Het volgende codevoorbeeld één entiteit worden opgehaald en controleert de **EmployeeCount** eigenschap bestaat voordat u het bijwerkt.  

```csharp
TableResult result =
        employeeTable.Execute(TableOperation.Retrieve(partitionKey, rowKey));
DynamicTableEntity department = (DynamicTableEntity)result.Result;

EntityProperty countProperty;

if (!department.Properties.TryGetValue("EmployeeCount", out countProperty))
{
        throw new
        InvalidOperationException("Invalid entity, EmployeeCount property not found.");
}
countProperty.Int32Value += 1;
employeeTable.Execute(TableOperation.Merge(department));  
```

### <a name="controlling-access-with-shared-access-signatures"></a>Beheren van toegang met handtekeningen voor gedeelde toegang
Shared Access Signature (SAS)-tokens kunt u toepassingen die client te wijzigen (en een query) tabelentiteiten rechtstreeks zonder de noodzaak om te verifiëren rechtstreeks met de tabelservice. Er zijn in principe drie belangrijke voordelen bij SAS gebruiken in uw toepassing:  

* U hoeft niet te distribueren naar een onbeveiligde platform (zoals een mobiel apparaat) sleutel van uw opslagaccount als u wilt toestaan dat het apparaat te openen en te wijzigen van de entiteiten in de tabel-service.  
* U kunt offload deel van het werk die web-en werkrollen uitvoeren bij het beheren van uw entiteiten op clientapparaten zoals eindgebruikers, computers en mobiele apparaten.  
* U kunt een beperkte toewijzen en tijd beperkt set machtigingen voor een client (zoals alleen-lezen toegang tot specifieke bronnen toe te staan).  

Zie voor meer informatie over het gebruik van SAS-tokens met de tabelservice [met behulp van Shared Access Signatures (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md).  

Echter, moet u nog steeds de SAS-tokens die de entiteiten in de tabelservice een clienttoepassing verlenen genereren: u moet dit doen in een omgeving met veilige toegang tot uw toegangscodes voor opslag. Normaal, gebruikt u een web- of worker-rol voor het genereren van de SAS-tokens en ervoor zorgen dat ze de clienttoepassingen die toegang nodig tot de entiteiten. Omdat er nog steeds een overhead voor het genereren en leveren van SAS-tokens op clients, moet u het beste aan deze overhead, met name in grootschalige scenario's verminderen.  

Het is mogelijk om een SAS-token die toegang tot een subset van de entiteiten in een tabel verleent te genereren. U maakt standaard een SAS-token voor een hele tabel, maar het is ook mogelijk om op te geven dat het SAS-token toegang verlenen tot een bereik van **PartitionKey** waarden of een bereik van **PartitionKey** en **RowKey** waarden. U kunt selecteren voor het genereren van SAS-tokens voor afzonderlijke gebruikers van uw systeem dat de SAS-token van elke gebruiker kan alleen ze toegang tot hun eigen entiteiten in de tabelservice.  

### <a name="asynchronous-and-parallel-operations"></a>Asynchrone en parallelle bewerkingen
Mits u kunt uw verzoeken om te worden verspreid over meerdere partities, kunt u de doorvoer en client reactiesnelheid verbeteren met behulp van asynchrone of parallelle query's.
U wellicht bijvoorbeeld twee of meer werkprocessen rolinstanties toegang krijgen tot uw tabellen parallel. U kunt afzonderlijke werkrollen die verantwoordelijk zijn voor bepaalde sets van partities hebben of gewoon hebben meerdere worker rolinstanties, elke toegang kunnen krijgen tot alle partities in een tabel.  

U kunt binnen een clientexemplaar doorvoer verbeteren door opslagbewerkingen asynchroon uitgevoerd. De Storage-clientbibliotheek kunt eenvoudig schrijven van asynchrone query's en wijzigingen. Bijvoorbeeld, kunt u bijvoorbeeld starten met de synchrone methode die alle entiteiten in een partitie haalt zoals weergegeven in de volgende C#-code:  

```csharp
private static void ManyEntitiesQuery(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);

        TableContinuationToken continuationToken = null;

        do
        {
        var employees = employeeTable.ExecuteQuerySegmented(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
    {
        ...
    }
        continuationToken = employees.ContinuationToken;
        } while (continuationToken != null);
}  
```

U kunt deze code eenvoudig wijzigen zodat de query wordt asynchroon als volgt uitgevoerd:  

```csharp
private static async Task ManyEntitiesQueryAsync(CloudTable employeeTable, string department)
{
        string filter = TableQuery.GenerateFilterCondition(
        "PartitionKey", QueryComparisons.Equal, department);
        TableQuery<EmployeeEntity> employeeQuery =
        new TableQuery<EmployeeEntity>().Where(filter);
        TableContinuationToken continuationToken = null;

        do
        {
        var employees = await employeeTable.ExecuteQuerySegmentedAsync(
                employeeQuery, continuationToken);
        foreach (var emp in employees)
        {
            ...
        }
        continuationToken = employees.ContinuationToken;
            } while (continuationToken != null);
}  
```

In dit voorbeeld asynchrone ziet u de volgende wijzigingen van de synchrone versie:  

* Handtekening van de methode bevat nu de **asynchrone** aanpassingsfunctie en retourneert een **taak** exemplaar.  
* In plaats van aanroepen de **ExecuteSegmented** methode voor het ophalen van de resultaten van de methode nu roept de **ExecuteSegmentedAsync** methode en gebruikt de **await** aanpassingsfunctie resultaten asynchroon ophalen.  

De clienttoepassing kan deze methode niet aanroepen meerdere keren (met verschillende waarden voor de **afdeling** parameter), en elke query wordt uitgevoerd op een afzonderlijke thread.  

Houd er rekening mee dat er geen asynchrone versie van is de **Execute** methode in de **TableQuery** klasse omdat de **IEnumerable** interface ondersteunt geen asynchrone opsomming.  

U kunt invoegen, bijwerken en verwijderen van entiteiten asynchroon. De volgende C#-voorbeeld ziet u een eenvoudige, synchrone methode invoegen of vervangen van een werknemer entiteit:  

```csharp
private static void SimpleEmployeeUpsert(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = employeeTable
        .Execute(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

U kunt deze code eenvoudig wijzigen zodat de update wordt asynchroon als volgt uitgevoerd:  

```csharp
private static async Task SimpleEmployeeUpsertAsync(CloudTable employeeTable,
        EmployeeEntity employee)
{
        TableResult result = await employeeTable
        .ExecuteAsync(TableOperation.InsertOrReplace(employee));
        Console.WriteLine("HTTP Status: {0}", result.HttpStatusCode);
}  
```

In dit voorbeeld asynchrone ziet u de volgende wijzigingen van de synchrone versie:  

* Handtekening van de methode bevat nu de **asynchrone** aanpassingsfunctie en retourneert een **taak** exemplaar.  
* In plaats van aanroepen de **Execute** methode voor het bijwerken van de entiteit, de methode nu roept de **ExecuteAsync** methode en gebruikt de **await** aanpassingsfunctie resultaten asynchroon ophalen.  

De clienttoepassing kan meerdere asynchrone methoden zoals deze aanroepen en elke methodeaanroep wordt uitgevoerd op een afzonderlijke thread.  

### <a name="credits"></a>Tegoed
Wij willen graag Bedankt dat de volgende leden van het team van Azure voor hun bijdragen: Dominic Betts, Jason Hogg, Jean Ghanem, Jai Haridas, Jeff Irwin, Vamshidhar Kommineni, Vinay Shah en Serdar Ozler evenals Tom Hollander van Microsoft DX. 

We willen ook graag Bedankt dat de volgende Microsoft MVP van voor hun waardevolle feedback in revisie runs: Igor Papirov en Edward Bakker.

[1]: ./media/storage-table-design-guide/storage-table-design-IMAGE01.png
[2]: ./media/storage-table-design-guide/storage-table-design-IMAGE02.png
[3]: ./media/storage-table-design-guide/storage-table-design-IMAGE03.png
[4]: ./media/storage-table-design-guide/storage-table-design-IMAGE04.png
[5]: ./media/storage-table-design-guide/storage-table-design-IMAGE05.png
[6]: ./media/storage-table-design-guide/storage-table-design-IMAGE06.png
[7]: ./media/storage-table-design-guide/storage-table-design-IMAGE07.png
[8]: ./media/storage-table-design-guide/storage-table-design-IMAGE08.png
[9]: ./media/storage-table-design-guide/storage-table-design-IMAGE09.png
[10]: ./media/storage-table-design-guide/storage-table-design-IMAGE10.png
[11]: ./media/storage-table-design-guide/storage-table-design-IMAGE11.png
[12]: ./media/storage-table-design-guide/storage-table-design-IMAGE12.png
[13]: ./media/storage-table-design-guide/storage-table-design-IMAGE13.png
[14]: ./media/storage-table-design-guide/storage-table-design-IMAGE14.png
[15]: ./media/storage-table-design-guide/storage-table-design-IMAGE15.png
[16]: ./media/storage-table-design-guide/storage-table-design-IMAGE16.png
[17]: ./media/storage-table-design-guide/storage-table-design-IMAGE17.png
[18]: ./media/storage-table-design-guide/storage-table-design-IMAGE18.png
[19]: ./media/storage-table-design-guide/storage-table-design-IMAGE19.png
[20]: ./media/storage-table-design-guide/storage-table-design-IMAGE20.png
[21]: ./media/storage-table-design-guide/storage-table-design-IMAGE21.png
[22]: ./media/storage-table-design-guide/storage-table-design-IMAGE22.png
[23]: ./media/storage-table-design-guide/storage-table-design-IMAGE23.png
[24]: ./media/storage-table-design-guide/storage-table-design-IMAGE24.png
[25]: ./media/storage-table-design-guide/storage-table-design-IMAGE25.png
[26]: ./media/storage-table-design-guide/storage-table-design-IMAGE26.png
[27]: ./media/storage-table-design-guide/storage-table-design-IMAGE27.png
[28]: ./media/storage-table-design-guide/storage-table-design-IMAGE28.png
[29]: ./media/storage-table-design-guide/storage-table-design-IMAGE29.png

