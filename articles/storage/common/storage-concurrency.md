---
title: Gelijktijdigheid beheren in Microsoft Azure Storage
description: Gelijktijdigheid van taken voor de services Blob, Queue, Table en bestand beheren
services: storage
documentationcenter: 
author: jasontang501
manager: tadb
editor: tysonn
ms.assetid: cc6429c4-23ee-46e3-b22d-50dd68bd4680
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.openlocfilehash: 937cca66a0af0674b868e6a87681adbea330e91c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Gelijktijdigheid beheren in Microsoft Azure Storage
## <a name="overview"></a>Overzicht
Moderne internetgebaseerde toepassingen hebben meestal meerdere gebruikers weergeven en gegevens tegelijkertijd bijwerken. U moet hiervoor toepassingsontwikkelaars zorgvuldig bedenken hoe u een voorspelbare om ervaring te bieden aan hun eindgebruikers, met name voor scenario's waarin meerdere gebruikers dezelfde gegevens kunnen bijwerken. Er zijn drie belangrijkste gegevens gelijktijdigheidsstrategieën die ontwikkelaars doorgaans overwegen:  

1. Optimistische gelijktijdigheid: een toepassing uitvoeren van die een update als onderdeel van de update controleert of de gegevens is gewijzigd sinds de toepassing die gegevens laatst gelezen. Bijvoorbeeld, als twee gebruikers een wiki-pagina een update op dezelfde pagina moeten vervolgens het wiki-platform moet ervoor zorgen dat de tweede update niet de eerste update – overschreven en dat de beide gebruikers begrijpen of de update is voltooid. Deze strategie wordt meestal gebruikt in webtoepassingen.
2. Volledige vergrendeling: een toepassing wilt uitvoeren van een update duurt een vergrendeling op een object te voorkomen dat andere gebruikers de gegevens worden bijgewerkt tot de vergrendeling is vrijgegeven. Bijvoorbeeld in een master/slave gegevens replicatiescenario waarbij alleen de master updates wordt uitgevoerd wordt het model doorgaans houdt een exclusieve vergrendeling voor langere tijd op de gegevens zodat niemand anders kunt bijwerken.
3. Laatste schrijver wins – een methode waarmee de bijwerkbewerkingen om door te gaan zonder te controleren als een andere toepassing heeft bijgewerkt de gegevens sinds de toepassing eerst de gegevens niet lezen. Deze strategie (of het ontbreken van een strategie voor een formele) wordt meestal gebruikt waarbij de gegevens zijn gepartitioneerd zodanig dat er geen kans dat meerdere gebruikers toegang krijgen dezelfde gegevens tot is. Dit kan ook nuttig zijn wanneer tijdelijke gegevensstromen wordt verwerkt.  

Dit artikel bevat een overzicht van hoe de Azure Storage-platform vereenvoudigt ook de ontwikkeling eerste-klas door ondersteuning te bieden voor alle drie deze strategieën gelijktijdigheid van taken.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure Storage: vereenvoudigt ook de ontwikkeling van de Cloud
De Azure storage-service ondersteunt alle drie strategieën, hoewel het onderscheidende in de mogelijkheid om aan te bieden volledige ondersteuning voor de optimistische en volledige gelijktijdigheid omdat deze is ontworpen om in te spelen van een model sterke consistentie wordt gegarandeerd dat wanneer de Storage-service een gegevens-insert voert of update-bewerking alle verdere toegang heeft tot aan gegevens ziet de nieuwste update van is. Opslagplatforms die gebruikmaken van een model uiteindelijke consistentie hebben een vertraging tussen wanneer een schrijfbewerking wordt uitgevoerd door één gebruiker en wanneer de bijgewerkte gegevens zijn zichtbaar voor andere gebruikers dus complicerende ontwikkeling van clienttoepassingen om inconsistenties te voorkomen dat van invloed zijn op eindgebruikers.  

Naast het selecteren van een gelijktijdigheidsstrategie voor een juiste moet ontwikkelaars ook rekening houden met hoe wijzigingen – met name wijzigingen aan hetzelfde object via transacties Hiermee isoleert u een opslag-platform. De Azure storage-service gebruikt snapshot-isolatie om toe te staan leesbewerkingen gebeurt als schrijfbewerkingen binnen één partitie. In tegenstelling tot andere isolatieniveaus snapshot-isolatie wordt gegarandeerd dat alle leesbewerkingen een consistente momentopname van de gegevens zien terwijl updates worden gegeven – in wezen door te retourneren van de laatste doorgevoerd waarden tijdens een update transactie verwerkt wordt.  

## <a name="managing-concurrency-in-blob-storage"></a>Gelijktijdigheid van taken in Blob storage beheren
U kunt ervoor kiezen voor het gebruik van beide modellen optimistische of volledige gelijktijdigheid van taken voor het beheren van toegang tot blobs en containers in de blob-service. Als u een strategie voor laatste schrijfbewerkingen wins niet expliciet opgeeft is de standaardinstelling.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistische gelijktijdigheid voor blobs en containers
De Storage-service wijst een id toe aan elk object dat is opgeslagen. Deze id wordt telkens bijgewerkt wanneer een updatebewerking wordt uitgevoerd op een object. De id wordt geretourneerd naar de client als onderdeel van een HTTP GET-antwoord met de ETag-header voor (entiteitscode) die is gedefinieerd in het HTTP-protocol. Een gebruiker een update uitvoeren op een dergelijk object kunt in de oorspronkelijke ETag samen met een voorwaardelijke kop verzenden om ervoor te zorgen dat een update vindt alleen plaats als een bepaalde voorwaarde is voldaan: in dit geval de voorwaarde is een header 'If-Match', waarvoor de Storage-Service-t o Zorg ervoor dat de waarde van de ETag die is opgegeven in de updateaanvraag is hetzelfde als die opgeslagen in de Storage-Service.  

Het overzicht van dit proces is als volgt:  

1. Een blob ophalen uit de storage-service, het antwoord bevat een HTTP-ETag-Header-waarde die aangeeft van de huidige versie van het object in de storage-service.
2. Wanneer u de blob bijwerkt, omvatten de ETag-waarde die u hebt ontvangen in stap 1 van de **If-Match** voorwaardelijke kop van de aanvraag die u naar de service verzendt.
3. De service vergelijkt de ETag-waarde in de aanvraag met de huidige ETag-waarde van de blob.
4. Als de huidige ETag-waarde van de blob is een verschillende versie dan de ETag in de **If-Match** voorwaardelijke kop in de aanvraag kan de service een 412-fout geretourneerd op de client. Hiermee geeft u aan de client of een ander proces de blob is bijgewerkt sinds deze de client opgehaald.
5. Als de huidige ETag-waarde van de blob is dezelfde versie als de ETag in de **If-Match** voorwaardelijke kop in de aanvraag kan de service de aangevraagde bewerking wordt uitgevoerd en updates van de huidige ETag-waarde van de blob om weer te geven dat deze een nieuwe versie gemaakt.  

De volgende C#-fragment (met behulp van de Storage-clientbibliotheek 4.2.0) toont een eenvoudig voorbeeld van het maken van een **If-Match AccessCondition** op basis van de ETag-waarde die toegankelijk is via de eigenschappen van een blob die is eerder opgehaald of ingevoegd. Vervolgens wordt de **AccessCondition** tijdens het bijwerken van de blob-object: de **AccessCondition** object voegt de **If-Match** header met de aanvraag. Wanneer de blob is bijgewerkt naar een ander proces, retourneert de blob-service een statusbericht HTTP 412 (voorwaarde is mislukt). U kunt het volledige voorbeeld hier downloaden: [beheren gelijktijdigheid van taken met behulp van Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve the ETag from the newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// to storage blob service which returns the etag in response.
string orignalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No etag, provided so orignal blob is overwritten (thus generating a new etag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try to update the blob using the orignal ETag provided when the blob was created
try
{
    Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(orignalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
        // TODO: client can decide on how it wants to handle the 3rd party updated content.
    }
    else
        throw;
}  
```

De Storage-Service biedt ook ondersteuning voor extra voorwaardelijke kopteksten, zoals **If-Modified-Since**, **If-Unmodified-Since** en **If-None-Match** evenals combinaties daarvan. Zie voor meer informatie [voorwaardelijke Headers opgeven voor Blob-servicebewerkingen](http://msdn.microsoft.com/library/azure/dd179371.aspx) op MSDN.  

De volgende tabel geeft een overzicht van de container-bewerkingen die voorwaardelijke kop, zoals accepteren **If-Match** in de aanvraag en die een ETag-waarde in het antwoord retourneren.  

| Bewerking | Container ETag-waarde retourneert | Voorwaardelijke kop accepteert |
|:--- |:--- |:--- |
| Container maken |Ja |Nee |
| Eigenschappen van Container ophalen |Ja |Nee |
| Container metagegevens ophalen |Ja |Nee |
| Metagegevens van de Container instellen |Ja |Ja |
| Container-ACL ophalen |Ja |Nee |
| Container-ACL instellen |Ja |Ja (*) |
| Verwijderen van Container |Nee |Ja |
| Lease-Container |Ja |Ja |
| Lijst met BLOB 's |Nee |Nee |

(*) De machtigingen die zijn gedefinieerd door SetContainerACL in de cache zijn opgeslagen en updates voor deze machtigingen 30 seconden duren doorgegeven tijdens de periode waarin updates zijn niet gegarandeerd consistent zijn.  

De volgende tabel geeft een overzicht van de blob-bewerkingen die voorwaardelijke kop, zoals accepteren **If-Match** in de aanvraag en die een ETag-waarde in het antwoord retourneren.

| Bewerking | ETag-waarde retourneert | Voorwaardelijke kop accepteert |
|:--- |:--- |:--- |
| Blob plaatsen |Ja |Ja |
| Ophalen van Blob |Ja |Ja |
| Blob-eigenschappen ophalen |Ja |Ja |
| Blob-eigenschappen instellen |Ja |Ja |
| Blobmetagegevens ophalen |Ja |Ja |
| Blobmetagegevens instellen |Ja |Ja |
| Lease-Blob (*) |Ja |Ja |
| Momentopname Blob |Ja |Ja |
| Blob kopiëren |Ja |Ja (voor een bron- en doelserver blob) |
| Blob kopiëren afbreken |Nee |Nee |
| Verwijderen van Blob |Nee |Ja |
| Blok plaatsen |Nee |Nee |
| Lijst met geblokkeerde websites plaatsen |Ja |Ja |
| Lijst met geblokkeerde websites ophalen |Ja |Nee |
| Pagina plaatsen |Ja |Ja |
| Get-paginabereiken |Ja |Ja |

(*) De ETag op een blob lease Blob niet gewijzigd.  

### <a name="pessimistic-concurrency-for-blobs"></a>Volledige vergrendeling voor blobs
Als u wilt vergrendelen van een blob voor exclusief gebruik, kunt u aanschaffen een [lease](http://msdn.microsoft.com/library/azure/ee691972.aspx) erop. Wanneer u een lease aanschaft, u opgeven hoe lang moet u de lease: dit kan zijn voor tussen 15 tot 60 seconden of oneindig die naar een exclusieve vergrendeling bedragen. U kunt een eindige lease te laten verlengen en u kunt een lease vrijgeven wanneer u klaar met het bent. De blob-service wordt automatisch eindig leases versies wanneer ze zijn verlopen.  

Leases schakelen verschillende synchronisatie strategieën om te worden ondersteund, met inbegrip van exclusieve schrijven / lezen, exclusieve schrijven gedeeld / exclusief lezen en schrijven gedeeld / exclusief lezen. Wanneer een lease bestaat de storage-service wordt afgedwongen exclusief schrijfbewerkingen (plaatsen, instellen en verwijderbewerkingen) exclusiviteit voor leesbewerkingen gezorgd moet echter de ontwikkelaar om ervoor te zorgen dat alle clienttoepassingen een lease-ID en die slechts één client tegelijk gebruiken heeft een geldige lease-ID. Leesbewerkingen die een lease-ID-resultaat niet in gedeelde leest opnemen.  

De volgende C#-fragment toont een voorbeeld van een exclusieve lease verkrijgen gedurende 30 seconden op een blob en vervolgens de lease los te laten bijwerken van de inhoud van de blob. Als er al een geldige lease op de blob wanneer u probeert een nieuwe lease te verkrijgen, retourneert de blob-service een resultaat van de status 'Conflict HTTP (409)'. Het volgende fragment maakt gebruik van een **AccessCondition** -object op voor de leasegegevens inkapselen wanneer een aanvraag voor het bijwerken van de blob in de storage-service op deze manier.  U kunt het volledige voorbeeld hier downloaden: [beheren gelijktijdigheid van taken met behulp van Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
// Acquire lease for 15 seconds
string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

// Update blob using lease. This operation will succeed
const string helloText = "Blob updated";
var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
blockBlob.UploadText(helloText, accessCondition: accessCondition);
Console.WriteLine("Blob updated using an exclusive lease");

//Simulate third party update to blob without lease
try
{
    // Below operation will fail as no valid lease provided
    Console.WriteLine("Trying to update blob without valid lease");
    blockBlob.UploadText("Update without lease, will fail");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
    else
        throw;
}  
```

Als u een schrijfbewerking voor een geleaste blob probeert zonder het doorgeven van de lease-ID, mislukt de aanvraag met een 412 fout. Houd er rekening mee dat als de lease is verlopen voordat de **UploadText** methode, maar u nog steeds doorgeven van de lease-ID, de aanvraag is ook mislukt met een **412** fout. Zie voor meer informatie over het beheren van verlopen leaseduur en lease id's, de [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST-documentatie.  

De volgende bewerkingen uit blob kunnen leases gebruiken voor het beheren van volledige vergrendeling:  

* Blob plaatsen
* Ophalen van Blob
* Blob-eigenschappen ophalen
* Blob-eigenschappen instellen
* Blobmetagegevens ophalen
* Blobmetagegevens instellen
* Verwijderen van Blob
* Blok plaatsen
* Lijst met geblokkeerde websites plaatsen
* Lijst met geblokkeerde websites ophalen
* Pagina plaatsen
* Get-paginabereiken
* Momentopname Blob - lease-ID is optioneel als er een lease bestaat
* Blob - lease ID is vereist als een lease op de bestemmings-blob bestaat kopiëren
* Afbreken kopie Blob - ID van de lease is vereist als er een oneindige lease op de bestemmings-blob bestaat
* Lease Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Volledige vergrendeling voor containers
Leases voor containers dezelfde strategieën synchronisatie wordt alleen ondersteund als op blobs inschakelen (exclusief voor schrijven / lezen, exclusieve schrijven gedeeld / exclusief lezen en schrijven gedeeld / lezen exclusief) maar in tegenstelling tot de blobs de storage-service alleen wordt afgedwongen exclusiviteit op delete-bewerkingen. Als u wilt verwijderen van een container met een actieve reservering geldt, moet een client de actieve lease-ID en de delete-aanvraag bevatten. Alle andere container bewerkingen slaagt in een container geleaste met inbegrip van de ID van de lease bewerkingen in dat geval ze worden gedeeld. Als exclusiviteit van update (put of set) of leesbewerkingen vereist vervolgens moet ontwikkelaars dat alle clients gebruiken een lease-ID en die alleen één client tegelijk een geldige lease-ID. heeft  

De volgende container bewerkingen kunnen leases gebruiken voor het beheren van volledige vergrendeling:  

* Verwijderen van Container
* Eigenschappen van Container ophalen
* Container metagegevens ophalen
* Metagegevens van de Container instellen
* Container-ACL ophalen
* Container-ACL instellen
* Lease-Container  

Zie voor meer informatie:  

* [Voorwaardelijke kop opgeven voor bewerkingen van de Blob-Service](http://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Lease-Container](http://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Lease Blob](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Het beheren van gelijktijdigheid van taken in de Tabelservice
De tabelservice gebruikt optimistische gelijktijdigheid controleert als standaardgedrag wanneer u werkt met entiteiten, in tegenstelling tot de blob-service waarbij u expliciet kiezen moet optimistische gelijktijdigheid controles uit te voeren. De andere verschil tussen de tabel en de blob-services is dat u alleen het gelijktijdigheid gedrag van entiteiten beheren kunt dat met de blob-service kunt u de gelijktijdigheid van zowel containers en blobs beheren.  

Moet worden gebruikt optimistische gelijktijdigheid, en om te controleren of een ander proces een entiteit gewijzigd, omdat u deze uit de tabel storage-service opgehaald, kunt u de ETag-waarde die u ontvangt als de tabelservice een entiteit retourneert. Het overzicht van dit proces is als volgt:  

1. Een entiteit ophalen uit de tabel storage-service, het antwoord bevat een ETag-waarde die aangeeft van de huidige id die is gekoppeld aan deze entiteit in de storage-service.
2. Wanneer u de entiteit bijwerkt, omvatten de ETag-waarde die u hebt ontvangen in stap 1 van de vereiste **If-Match** koptekst van de aanvraag die u naar de service verzendt.
3. De service worden vergeleken met de huidige ETag-waarde van de entiteit de ETag-waarde in de aanvraag.
4. Als de huidige ETag-waarde van de entiteit anders dan de ETag in het verplichte is **If-Match** -header in de aanvraag kan de service een 412-fout geretourneerd op de client. Hiermee geeft u aan de client of een ander proces de entiteit is bijgewerkt sinds deze de client opgehaald.
5. Als de huidige ETag-waarde van de entiteit hetzelfde als de ETag in het verplichte is **If-Match** header in de aanvraag of de **If-Match** header bevat het jokerteken (*), de service de aangevraagde bewerking wordt uitgevoerd en updates van de huidige ETag-waarde van de entiteit om weer te geven dat deze is bijgewerkt.  

Opmerking dat in tegenstelling tot de blob-service de tabelservice vereist dat de client wilt opnemen een **If-Match** header in updateaanvragen. Het is echter mogelijk om af te dwingen een onvoorwaardelijke bijwerken (laatste schrijver strategie voor wins) en gelijktijdigheid controles overslaan als de client stelt de **If-Match** header aan het jokerteken (*) in de aanvraag.  

De volgende C#-fragment toont klantentiteit die eerder gemaakt of opgehaald met hun e-mailadres dat is bijgewerkt. De eerste plaats of bewerking slaat de ETag-waarde in het object van de klant ophalen en omdat het voorbeeld wordt hetzelfde objectexemplaar gebruikt bij het uitvoeren van de vervangingsbewerking, het verzendt automatisch de ETag-waarde terug naar de tabelservice, het inschakelen van de service om te controleren op schendingen van gelijktijdigheid. Wanneer de entiteit in de table storage is bijgewerkt naar een ander proces, stuurt de service een statusbericht HTTP 412 (voorwaarde is mislukt).  U kunt het volledige voorbeeld hier downloaden: [beheren gelijktijdigheid van taken met behulp van Azure Storage](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
try
{
    customer.Email = "updatedEmail@contoso.org";
    TableOperation replaceCustomer = TableOperation.Replace(customer);
    customerTable.Execute(replaceCustomer);
    Console.WriteLine("Replace operation succeeded.");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == 412)
        Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
    else
        throw;
}  
```

Als u wilt de gelijktijdigheidscontrole expliciet uitschakelen, moet u instellen de **ETag** eigenschap van de **werknemer** object naar ' * ' voordat u de vervangingsbewerking uitvoert.  

```csharp
customer.ETag = "*";  
```

De volgende tabel ziet u hoe de entiteit tabelbewerkingen ETag waarden gebruiken:

| Bewerking | ETag-waarde retourneert | Aanvraag-header If-Match vereist |
|:--- |:--- |:--- |
| Query-entiteiten |Ja |Nee |
| Entiteit invoegen |Ja |Nee |
| Bijwerken van entiteit |Ja |Ja |
| Samenvoegen van entiteit |Ja |Ja |
| Entiteit verwijderen |Nee |Ja |
| Invoegen of vervangen van entiteit |Ja |Nee |
| Invoegen of samenvoegen van entiteit |Ja |Nee |

Houd er rekening mee dat de **invoegen of vervangen entiteit** en **invoegen of samenvoegen entiteit** bewerkingen uitvoeren *niet* gelijktijdigheid controles uitvoeren, omdat ze een ETag-waarde niet aan de tabelservice verzendt.  

In het algemeen verstandig ontwikkelaars met tabellen optimistische gelijktijdigheid van taken bij het ontwikkelen van schaalbare toepassingen. Als volledige vergrendeling is vereist, kunnen een aanpak ontwikkelaars duren wanneer de toegang tot tabellen wordt Wijs een aangewezen blob voor elke tabel en probeer het een lease op het blob moet uitvoeren voordat het besturingssysteem op de tabel. Deze benadering is de toepassing om te controleren of alle gegevenspaden toegang verkrijgen van de lease voordat het besturingssysteem op de tabel vereist. Vergeet niet dat de minimale leasetijd 15 seconden dat moet zorgvuldig gebeuren voor schaalbaarheid is.  

Zie voor meer informatie:  

* [Bewerkingen op entiteiten](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Het beheren van gelijktijdigheid van taken in de Queue-Service
Een scenario die in welke gelijktijdigheid van taken een probleem in de wachtrij-service is is waar meerdere clients zijn bij het ophalen van berichten uit een wachtrij. Wanneer een bericht wordt opgehaald uit de wachtrij, wordt het antwoord bevat het bericht en een ontvangstwaarde pop, dit is vereist voor het verwijderen van het bericht. Het bericht wordt niet automatisch verwijderd uit de wachtrij, maar nadat zijn opgehaald, het is niet zichtbaar voor andere clients voor de tijdsinterval die is opgegeven door de parameter visibilitytimeout. De client die het bericht haalt wordt het bericht wilt verwijderen nadat deze is verwerkt en vóór de tijd die is opgegeven door de TimeNextVisible element van het antwoord, die wordt berekend op basis van de waarde van de parameter visibilitytimeout verwacht. De waarde van visibilitytimeout is toegevoegd aan de tijd waarop het bericht om te bepalen van de waarde van TimeNextVisible wordt opgehaald.  

De queue-service heeft geen ondersteuning voor volledige noch optimistische gelijktijdigheid, en voor deze berichten worden verwerkt op een manier idempotent van reden clients verwerken van berichten die zijn opgehaald uit een wachtrij moet. Een strategie voor wins van laatste schrijver wordt gebruikt voor de update-bewerkingen zoals SetQueueServiceProperties, SetQueueMetaData, SetQueueACL en UpdateMessage.  

Zie voor meer informatie:  

* [REST-API van wachtrij](http://msdn.microsoft.com/library/azure/dd179363.aspx)
* [Berichten ophalen](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Het beheren van gelijktijdigheid van taken in de File-Service
De file-service zijn toegankelijk via twee verschillende protocoleindpunten – SMB en REST. De REST-service biedt geen ondersteuning voor beperkte vergrendeling of volledige vergrendeling en alle updates moeten een strategie voor wins van laatste schrijver volgen. Bestand systeem vergrendelen mechanismen voor het beheren van toegang tot gedeelde bestanden, inclusief de mogelijkheid om uit te voeren volledige vergrendeling kunnen gebruikmaken van SMB-clients die bestandsshares koppelen. Wanneer een SMB-client een bestand opent, wordt Hiermee wordt de toegang tot het bestand en de share modus. Een optie voor toegang tot het bestand 'Schrijven' of ' Lezen/schrijven' samen met een bestandsshare-modus, 'None' leidt tot het bestand wordt vergrendeld door een SMB-client tot het bestand wordt gesloten. Als REST-bewerking wordt uitgevoerd voor een bestand waarin een SMB-client het bestand vergrendeld heeft wordt in de REST-service statuscode 409 (Conflict) met foutcode SharingViolation geretourneerd.  

Wanneer een bestand om te verwijderen van een SMB-client wordt geopend, wordt het bestand gemarkeerd als in behandeling verwijderen tot de SMB-client open ingangen in dat bestand gesloten. Wanneer een bestand is gemarkeerd als in behandeling verwijderen, wordt een REST-bewerking op dat bestand statuscode 409 (Conflict) met foutcode SMBDeletePending geretourneerd. Statuscode 404 (niet gevonden) wordt niet geretourneerd omdat het is mogelijk dat de SMB-client om de vlag in afwachting van verwijdering vóór het sluiten van het bestand te verwijderen. Met andere woorden, worden statuscode 404 (niet gevonden) alleen verwacht wanneer het bestand is verwijderd. Houd er rekening mee dat wanneer een bestand in een SMB verwijderen status in behandeling is, deze niet in de lijst met bestanden resultaten opgenomen worden. Merk ook op dat de REST-bestand verwijderen en REST verwijderen Directory bewerkingen moment toegepast zijn en niet in een status in afwachting van verwijdering resulteren.  

Zie voor meer informatie:  

* [Hiermee vergrendelt u het beheer van bestand](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Samenvatting en de volgende stappen
De Microsoft Azure Storage-service is ontworpen om te voldoen aan de behoeften van de meest complexe on line toepassingen zonder ontwikkelaars aangetast of denken over belangrijke ontwerp veronderstellingen zoals consistentie gelijktijdigheid van taken en gegevens die ze afkomstig zijn moet worden ondernomen bij verleend.  

Voor de volledige voorbeeldtoepassing waarnaar wordt verwezen in deze blog:  

* [Gelijktijdigheid van taken met behulp van Azure Storage - voorbeeldtoepassing beheren](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Voor meer informatie over Azure Storage-Zie:  

* [Startpagina van Microsoft Azure Storage](https://azure.microsoft.com/services/storage/)
* [Inleiding tot Azure Storage](storage-introduction.md)
* Opslag aan de slag voor [Blob](../blobs/storage-dotnet-how-to-use-blobs.md), [tabel](../../cosmos-db/table-storage-how-to-use-dotnet.md), [wachtrijen](../storage-dotnet-how-to-use-queues.md), en [bestanden](../storage-dotnet-how-to-use-files.md)
* Opslagarchitectuur – [Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

