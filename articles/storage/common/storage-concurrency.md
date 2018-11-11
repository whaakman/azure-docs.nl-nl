---
title: Gelijktijdigheid beheren in Microsoft Azure Storage
description: Over het beheren van de waarde voor concurrency voor de services Blob, wachtrij, tabel en bestand
services: storage
author: jasontang501
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.component: common
ms.openlocfilehash: 25de4f28d7516f5c7830b24e4c999ceb855a7759
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242973"
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Gelijktijdigheid beheren in Microsoft Azure Storage
## <a name="overview"></a>Overzicht
Internet op basis van moderne toepassingen hebben doorgaans meerdere gebruikers weergeven en bijwerken van gegevens tegelijkertijd. Dit is vereist voor ontwikkelaars van toepassingen moet u zorgvuldig nadenken over hoe u een voorspelbare ervaring bieden aan eindgebruikers, met name voor scenario's waarin meerdere gebruikers dezelfde gegevens kunnen bijwerken. Er zijn drie strategieën van de belangrijkste gegevens gelijktijdigheid die ontwikkelaars gewoonlijk overwegen:  

1. Optimistische gelijktijdigheid: een toepassing uitvoeren van die een update als onderdeel van de update controleren wordt of de gegevens is gewijzigd sinds de toepassing die gegevens laatst gelezen. Bijvoorbeeld, als twee gebruikers die een wikipagina bekijken moeten u een update op dezelfde pagina vervolgens de wiki-platform moet ervoor zorgen dat de tweede update niet de eerste update – overschreven en dat beide gebruikers begrijpen of de update is voltooid of niet. Deze strategie wordt meestal gebruikt in webtoepassingen.
2. Pessimistische gelijktijdigheid: een toepassing wilt uitvoeren van een update duurt een vergrendeling op een object te voorkomen dat andere gebruikers van de gegevens worden bijgewerkt, totdat de vergrendeling is uitgebracht. Bijvoorbeeld in een master/slave gegevens replicatiescenario waarbij alleen de master updates wordt uitgevoerd wordt het model doorgaans houdt een exclusieve vergrendeling voor langere tijd op de gegevens om ervoor te zorgen dat niemand anders het kunnen bijwerken.
3. Laatste schrijver wins – een benadering waarmee een update-bewerkingen om door te gaan zonder te controleren als elke andere toepassing heeft de gegevens bijgewerkt sinds de toepassing eerst de gegevens niet lezen. Deze strategie (of het ontbreken van een strategie voor een formele) wordt meestal gebruikt waarbij de gegevens zijn gepartitioneerd zodanig dat er geen kans is dat meerdere gebruikers toegang krijgen dezelfde gegevens tot. Het kan ook nuttig zijn waar tijdelijke gegevensstromen worden verwerkt.  

Dit artikel bevat een overzicht van hoe het Azure Storage-platform vereenvoudigt de ontwikkeling biedt een eersteklas ondersteuning voor alle drie deze gelijktijdigheidsstrategieën.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure Storage – vereenvoudigt de ontwikkeling van Cloud
De Azure storage-service biedt ondersteuning voor alle drie strategieën, hoewel dit onderscheidend in de mogelijkheid om u te bieden volledige ondersteuning voor optimistische en Pessimistische gelijktijdigheid omdat deze is ontworpen voor het gebruik van een sterke consistentie-model dat zorgt ervoor dat wanneer de Storage-service een insert gegevens doorgevoerd of update-bewerking alle verdere werkzaamheden gegevens ziet de meest recente update. Opslagplatforms die gebruikmaken van een model voor uiteindelijke consistentie hebben een vertraging tussen als een schrijfbewerking wordt uitgevoerd door één gebruiker en wanneer de bijgewerkte gegevens kunnen worden weergegeven door andere gebruikers dus complicerende ontwikkeling van clienttoepassingen om te voorkomen dat inconsistenties van die betrekking hebben op eindgebruikers.  

Naast het selecteren van een juiste gelijktijdigheid-strategie moeten ontwikkelaars ook rekening houden met hoe wijzigingen, met name wijzigingen aan hetzelfde object voor transacties worden geïsoleerd in een opslagplatform. De Azure storage-service maakt gebruik van snapshot-isolatie om toe te staan van leesbewerkingen worden uitgevoerd als schrijfbewerkingen binnen een enkele partitie. In tegenstelling tot andere niveaus isolatieniveau snapshot-isolatie zorgt ervoor dat alle leesbewerkingen een toepassingsconsistente momentopname van de gegevens zien, zelfs als er updates worden gegeven – in wezen door te retourneren van de laatst vastgelegde waarden tijdens een update transactie wordt verwerkt.  

## <a name="managing-concurrency-in-blob-storage"></a>Gelijktijdigheid beheren in Blob-opslag
U kunt kiezen voor het gebruik van een van beide modellen kiezen voor optimistische of Pessimistische gelijktijdigheid voor het beheren van toegang tot blobs en containers in de blob-service. Als u een strategie voor laatste schrijfbewerkingen wins niet expliciet opgeeft is de standaardinstelling.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Optimistische gelijktijdigheid voor blobs en containers
De Storage-service wijst een id toe aan elk object dat is opgeslagen. Deze id wordt bijgewerkt telkens wanneer een updatebewerking wordt uitgevoerd op een object. De id wordt geretourneerd naar de client als onderdeel van een HTTP GET-antwoord met de ETag (entiteitscode)-header die is gedefinieerd in het HTTP-protocol. Een gebruiker een update uitvoeren op dergelijk object kunt verzenden in de oorspronkelijke ETag samen met een voorwaardelijke header om ervoor te zorgen dat een update wordt alleen uitgevoerd als een bepaalde voorwaarde is voldaan: in dit geval de voorwaarde is een header 'If-None-Match', waarvoor de Storage-Service-t o Zorg ervoor dat de waarde van de ETag die is opgegeven in de updateaanvraag is hetzelfde als die die zijn opgeslagen in de Storage-Service.  

De omtrek van dit proces is als volgt:  

1. Een blob ophalen uit de storage-service, het antwoord bevat een HTTP-Header voor ETag-waarde die aangeeft in de huidige versie van het object in de storage-service.
2. Wanneer u de blob bijwerkt, bevatten de ETag-waarde die u hebt ontvangen in stap 1 van de **If-Match** voorwaardelijke kop van de aanvraag die u naar de service verzendt.
3. De service vergelijkt de ETag-waarde in de aanvraag met de huidige ETag-waarde van de blob.
4. Als de huidige ETag-waarde van de blob is een andere versie dan de ETag in de **If-Match** voorwaardelijke-header in de aanvraag, de service retourneert een 412 fout naar de client. Hiermee geeft u aan de client of een ander proces de blob is bijgewerkt, omdat deze wordt opgehaald door de client.
5. Als de huidige ETag-waarde van de blob is dezelfde versie als de ETag in de **If-Match** voorwaardelijke koptekst in de aanvraag, de service de aangevraagde bewerking wordt uitgevoerd en werkt de huidige ETag-waarde van de blob om weer te geven dat deze is gemaakt een nieuwe versie.  

De volgende C#-fragment (met behulp van de Storage-clientbibliotheek 4.2.0) toont een eenvoudig voorbeeld van hoe u een **If-Match AccessCondition** op basis van de ETag-waarde die toegankelijk is vanuit de eigenschappen van een blob die eerder is opgehaald of worden geplaatst. Vervolgens wordt de **AccessCondition** tijdens het bijwerken van de blob-object: de **AccessCondition** object wordt toegevoegd de **If-Match** header aan de aanvraag. Als een ander proces wordt de blob is bijgewerkt, retourneert de blob-service een statusbericht HTTP 412 (voorwaarde niet voldaan). U kunt het volledige voorbeeld hier downloaden: [gelijktijdigheid van beheren met behulp van Azure Storage](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve the ETag from the newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// to storage blob service which returns the ETag in response.
string originalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No ETag provided so original blob is overwritten (thus generating a new ETag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try to update the blob using the original ETag provided when the blob was created
try
{
    Console.WriteLine("Trying to update blob using original ETag to generate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(originalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's original ETag no longer matches");
        // TODO: client can decide on how it wants to handle the 3rd party updated content.
    }
    else
        throw;
}  
```

De Storage-Service biedt ook ondersteuning voor extra kopteksten die voorwaardelijke zoals **als gewijzigd sinds**, **als niet gewijzigd sinds** en **If-None-Match** , evenals combinaties daarvan. Zie voor meer informatie, [voorwaardelijke Headers op te geven voor Blob-servicebewerkingen](https://msdn.microsoft.com/library/azure/dd179371.aspx) op MSDN.  

De volgende tabel geeft een overzicht van de container-bewerkingen die voorwaardelijke headers zoals accepteren **If-Match** in de aanvraag en die een ETag-waarde in het antwoord retourneren.  

| Bewerking | Container ETag-waarde geretourneerd | Voorwaardelijke headers accepteert |
|:--- |:--- |:--- |
| Container maken |Ja |Nee |
| Eigenschappen van de Container ophalen |Ja |Nee |
| De Containermetagegevens van de ophalen |Ja |Nee |
| De Containermetagegevens van de instellen |Ja |Ja |
| Container ACL ophalen |Ja |Nee |
| Container ACL instellen |Ja |Ja (*) |
| Container verwijderen |Nee |Ja |
| Lease-Container |Ja |Ja |
| Lijst met Blobs |Nee |Nee |

(*) De machtigingen die zijn gedefinieerd door SetContainerACL in de cache worden opgeslagen en updates om deze machtigingen aan te nemen 30 seconden worden doorgegeven tijdens de periode waarin updates niet worden gegarandeerd consistent.  

De volgende tabel geeft een overzicht van de blob-bewerkingen die voorwaardelijke headers zoals accepteren **If-Match** in de aanvraag en die een ETag-waarde in het antwoord retourneren.

| Bewerking | ETag-waarde geretourneerd | Voorwaardelijke headers accepteert |
|:--- |:--- |:--- |
| Blob plaatsen |Ja |Ja |
| Blob ophalen |Ja |Ja |
| Blobeigenschappen ophalen |Ja |Ja |
| Blobeigenschappen instellen |Ja |Ja |
| De metagegevens van de blob ophalen |Ja |Ja |
| Instellen van de metagegevens van de Blob |Ja |Ja |
| Lease-Blob (*) |Ja |Ja |
| Momentopname maken van Blob |Ja |Ja |
| Blob kopiëren |Ja |Ja (voor een bron- en blob) |
| Kopiëren van de Blob afbreken |Nee |Nee |
| Blob verwijderen |Nee |Ja |
| Blok plaatsen |Nee |Nee |
| Lijst met geblokkeerde websites plaatsen |Ja |Ja |
| Lijst met geblokkeerde websites ophalen |Ja |Nee |
| Pagina plaatsen |Ja |Ja |
| Get Page-bereiken |Ja |Ja |

(*) Lease-Blob verandert niet de ETag voor een blob.  

### <a name="pessimistic-concurrency-for-blobs"></a>Pessimistische gelijktijdigheid voor blobs
Als u wilt vergrendelen van een blob voor exclusief gebruik, kunt u verkrijgen een [lease](https://msdn.microsoft.com/library/azure/ee691972.aspx) erop. Wanneer u een lease verkrijgen, geeft u voor hoe lang moet u de lease: dit kan zijn voor tussen 15 tot 60 seconden of oneindig, die in een exclusieve vergrendeling. U kunt een eindige lease te laten verlengen en u kunt een lease vrijgeven wanneer u klaar met het bent. De blob-service wordt automatisch eindige leases versies wanneer ze zijn verlopen.  

Leases schakelen strategieën voor verschillende synchronisatie worden ondersteund, met inbegrip van exclusieve schrijven / gedeelde lezen, exclusieve schrijven / exclusieve lezen en schrijven gedeeld / exclusieve lezen. Wanneer een lease bestaat exclusieve schrijfbewerkingen (put, instellen en verwijderbewerkingen) wordt afgedwongen door de storage-service echter het ervoor te zorgen dat de exclusiviteit voor leesbewerkingen is vereist voor de ontwikkelaar om ervoor te zorgen dat alle clienttoepassingen een lease-ID gebruiken en die slechts één client tegelijk heeft een geldige lease-ID. Leesbewerkingen die een lease-ID resultaat niet in gedeelde leesbewerkingen opnemen.  

De volgende C#-codefragment toont een voorbeeld van een exclusieve lease verkrijgen gedurende 30 seconden voor een blob, het bijwerken van de inhoud van de blob en vervolgens het vrijgeven van de lease. Als er al een geldige lease op de blob wanneer u probeert een nieuwe lease te verkrijgen, retourneert de blob-service een resultaat van de status 'HTTP (409) Conflict'. Het volgende fragment wordt een **AccessCondition** object om in te kapselen van de lease-informatie wanneer deze een aanvraag voor het bijwerken van de blob in de storage-service maakt.  U kunt het volledige voorbeeld hier downloaden: [gelijktijdigheid van beheren met behulp van Azure Storage](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Als u een schrijfbewerking voor een lease blob zonder de lease-ID, mislukt de aanvraag met een 412-fout. Houd er rekening mee dat als de lease is verlopen voordat de **UploadText** methode, maar u nog steeds de lease-ID doorgeven, de aanvraag is ook mislukt met een **412** fout. Zie voor meer informatie over het beheren van de lease verlopen en de lease-id's, de [Lease Blob](https://msdn.microsoft.com/library/azure/ee691972.aspx) REST-documentatie.  

De volgende blob-bewerkingen kunnen leases gebruiken voor het beheren van Pessimistische gelijktijdigheid:  

* Blob plaatsen
* Blob ophalen
* Blobeigenschappen ophalen
* Blobeigenschappen instellen
* De metagegevens van de blob ophalen
* Instellen van de metagegevens van de Blob
* Blob verwijderen
* Blok plaatsen
* Lijst met geblokkeerde websites plaatsen
* Lijst met geblokkeerde websites ophalen
* Pagina plaatsen
* Get Page-bereiken
* Momentopname maken van Blob - lease-ID is optioneel als er een lease bestaat
* Kopiëren van de Blob - lease-ID vereist als een lease op de bestemmings-blob bestaat
* Afbreken kopiëren van de Blob - lease-ID is vereist als er een oneindige lease bestaat op de bestemmings-blob
* Lease-Blob  

### <a name="pessimistic-concurrency-for-containers"></a>Pessimistische gelijktijdigheid voor containers
Lease op containers dezelfde synchronisatie-strategieën worden ondersteund als op blobs inschakelen (exclusief voor schrijven / gedeelde lezen, exclusieve schrijven / exclusieve lezen en schrijven gedeeld / lezen exclusief) maar in tegenstelling tot de blobs de storage-service alleen afgedwongen exclusiviteit op verwijderen van bewerkingen. Als u wilt verwijderen van een container met een actieve lease, een client moet de actieve lease-ID opnemen in de delete-aanvraag. Alle andere bewerkingen container slagen voor een lease container zonder op te nemen van de lease-ID in dat geval zijn gedeelde bewerkingen. Als de exclusiviteit van update (put of set) of leesbewerkingen vereist is vervolgens ontwikkelaars moeten controleert u of dat alle clients gebruiken een lease-ID en die alleen één client tegelijk een geldige lease-ID heeft.  

De volgende bewerkingen voor de container kunnen leases gebruiken voor het beheren van Pessimistische gelijktijdigheid:  

* Container verwijderen
* Eigenschappen van de Container ophalen
* De Containermetagegevens van de ophalen
* De Containermetagegevens van de instellen
* Container ACL ophalen
* Container ACL instellen
* Lease-Container  

Zie voor meer informatie:  

* [Voorwaardelijke Headers op te geven voor bewerkingen van Blob Service](https://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Lease-Container](https://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Lease-Blob ](https://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Gelijktijdigheid beheren in de Table-Service
De tabelservice gebruikt optimistische gelijktijdigheid controleert als standaardgedrag wanneer u werkt met entiteiten, in tegenstelling tot de blob-service waar u expliciet kiezen moet optimistische gelijktijdigheid controles uit te voeren. De andere verschil tussen de tabel- en blob-services is dat u alleen de gelijktijdigheid-gedrag van entiteiten beheren kunt terwijl met de blob-service kunt u de waarde voor concurrency van containers en blobs beheren.  

Optimistische gelijktijdigheid gebruiken en om te controleren als een ander proces een entiteit gewijzigd, omdat u deze hebt opgehaald uit de table storage-service, kunt u de ETag-waarde die u ontvangt wanneer de table-service een entiteit wordt. De omtrek van dit proces is als volgt:  

1. Een entiteit ophalen met de table storage-service, het antwoord bevat een ETag-waarde die aangeeft van de huidige id die is gekoppeld aan die entiteit in de storage-service.
2. Wanneer u de entiteit bijwerkt, bevatten de ETag-waarde die u hebt ontvangen in stap 1 van de verplichte **If-Match** -header van de aanvraag die u naar de service verzendt.
3. De service vergelijkt de ETag-waarde in de aanvraag met de huidige ETag-waarde van de entiteit.
4. Als de huidige ETag-waarde van de entiteit anders dan de ETag in het verplichte is **If-Match** header in de aanvraag, de service retourneert een 412 fout naar de client. Hiermee geeft u aan de client of een ander proces de entiteit is bijgewerkt, omdat deze wordt opgehaald door de client.
5. Als de huidige ETag-waarde van de entiteit hetzelfde als de ETag in het verplichte is **If-Match** -header in de aanvraag of de **If-Match** header bevat het jokerteken (*), voert de service uit de de aangevraagde bewerking en werkt de huidige ETag-waarde van de entiteit om weer te geven dat deze is bijgewerkt.  

Houd er rekening mee dat in tegenstelling tot de blob-service, de table-service vereist dat de client om op te nemen een **If-Match** -header in verzoeken om te werken. Het is echter mogelijk om af te dwingen een onvoorwaardelijk update (laatste schrijver WINS-strategie) en gelijktijdigheid controles overslaan als de client stelt de **If-Match** header aan het jokerteken (*) in de aanvraag.  

De volgende C#-fragment toont een klantentiteit die u eerder hebt gemaakt of opgehaald met hun e-mailadres dat is bijgewerkt. De eerste plaats of bewerking winkels de ETag-waarde in de klant-object ophalen omdat in het voorbeeld dezelfde objectinstantie wordt bij het uitvoeren van de vervangingsbewerking, verzonden en wordt automatisch de ETag-waarde terug naar de tabelservice, de service inschakelen Controleer of schendingen van gelijktijdigheid. Wanneer een ander proces is bijgewerkt naar de entiteit in de tabelopslag, retourneert de service een statusbericht HTTP 412 (voorwaarde niet voldaan).  U kunt het volledige voorbeeld hier downloaden: [gelijktijdigheid van beheren met behulp van Azure Storage](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Als u wilt de gelijktijdigheidscontrole van de expliciet uitschakelen, moet u instellen de **ETag** eigenschap van de **werknemer** object toe aan ' * ' voordat u de vervangingsbewerking niet uitvoeren.  

```csharp
customer.ETag = "*";  
```

De volgende tabel geeft een overzicht van hoe de entiteit tabelbewerkingen ETag waarden gebruiken:

| Bewerking | ETag-waarde geretourneerd | If-Match-aanvraagheader is vereist |
|:--- |:--- |:--- |
| Entiteiten opvragen |Ja |Nee |
| Entiteit invoegen |Ja |Nee |
| Een entiteit bijgewerkt |Ja |Ja |
| Entiteit samenvoegen |Ja |Ja |
| Entiteit verwijderen |Nee |Ja |
| Entiteit invoegen of vervangen |Ja |Nee |
| Entiteit invoegen of samenvoegen |Ja |Nee |

Houd er rekening mee dat de **invoegen of vervangen entiteit** en **invoegen of samenvoegen entiteit** bewerkingen uitvoeren *niet* gelijktijdigheid controles niet uitvoeren omdat ze een ETag-waarde niet naar de tabel verzenden de service.  

In het algemeen afhankelijk ontwikkelaars die gebruikmaken van tabellen zijn van optimistische gelijktijdigheid bij het ontwikkelen van schaalbare toepassingen. Als pessimistische vergrendeling is vereist, wordt een aanpak ontwikkelaars kunnen uitvoeren wanneer toegang tot tabellen is het toewijzen van een aangewezen blob voor elke tabel en probeer een lease op de blob moet uitvoeren voordat het besturingssysteem op de tabel. Deze aanpak is vereist voor de toepassing om te controleren of alle gegevenspaden toegang verkrijgen van de lease voordat het besturingssysteem op de tabel. U moet ook Houd er rekening mee dat de minimale leaseperiode is 15 seconden waarvoor is vereist zorgvuldige overweging voor schaalbaarheid.  

Zie voor meer informatie:  

* [Bewerkingen voor entiteiten](https://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Gelijktijdigheid beheren in de Queue-Service
Een scenario die in welke gelijktijdigheid een probleem in de Queue-service is is waar meerdere clients zijn bij het ophalen van berichten uit een wachtrij. Wanneer een bericht wordt opgehaald uit de wachtrij, wordt het antwoord bevat het bericht en een pop-ontvangstbericht-waarde, die is vereist voor het verwijderen van het bericht. Het bericht wordt niet automatisch verwijderd uit de wachtrij, maar nadat deze is opgehaald, is niet zichtbaar voor andere clients voor het tijdsinterval dat is opgegeven door de parameter visibilitytimeout. De client die het bericht haalt wordt het bericht verwijderd nadat deze is verwerkt, en voordat u de tijd die is opgegeven door de TimeNextVisible element van het antwoord, die wordt berekend op basis van de waarde van de parameter visibilitytimeout verwacht. De waarde van visibilitytimeout wordt toegevoegd aan de tijd waarop het bericht om te bepalen van de waarde van TimeNextVisible wordt opgehaald.  

De queue-service heeft geen ondersteuning voor optimistische of Pessimistische gelijktijdigheid en voor deze reden clients verwerken van berichten die worden opgehaald uit een wachtrij moeten Controleer of de berichten worden verwerkt op een manier idempotent zijn. Een strategie voor een laatste schrijver wins wordt gebruikt voor update-bewerkingen, zoals SetQueueServiceProperties, SetQueueMetaData, SetQueueACL en UpdateMessage.  

Zie voor meer informatie:  

* [REST-API voor Queue-Service](https://msdn.microsoft.com/library/azure/dd179363.aspx)
* [Berichten ophalen](https://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Gelijktijdigheid beheren in de File-Service
De file-service kan worden geopend met behulp van twee verschillende protocoleindpunten: SMB- als REST. De REST-service heeft geen ondersteuning voor optimistische vergrendeling of pessimistische vergrendeling en alle updates volgen een laatste schrijver WINS-strategie. SMB-clients die koppelen van bestandsshares kunnen gebruikmaken van bestand system vergrendelen mechanismen voor het beheren van toegang tot gedeelde bestanden, inclusief de mogelijkheid om uit te voeren pessimistische vergrendeling. Wanneer een bestand wordt geopend in een SMB-client, wordt Hiermee geeft u de toegang tot het bestand en de share modus. Instellen van een optie toegang tot het bestand van 'Schrijven' of ' Lezen/schrijven' samen met een bestandsshare-modus, van 'None' resulteert in het bestand wordt vergrendeld door een SMB-client totdat het bestand wordt gesloten. Als de REST-bewerking wordt uitgevoerd op een bestand waarin een SMB-client het bestand vergrendeld heeft wordt de REST-service-statuscode 409 (Conflict) met foutcode SharingViolation geretourneerd.  

Wanneer een bestand voor het verwijderen van een SMB-client wordt geopend, wordt het bestand gemarkeerd als in behandeling verwijderen totdat alle andere SMB-client open ingangen in dat bestand gesloten. Wanneer een bestand is gemarkeerd als in behandeling verwijderen, wordt een REST-bewerking op dat bestand statuscode 409 (Conflict) met foutcode SMBDeletePending geretourneerd. Statuscode 404 (niet gevonden) wordt niet geretourneerd omdat het mogelijk dat de SMB-client om de verwijdering is in behandeling vlag vóór het sluiten van het bestand te verwijderen. Met andere woorden: statuscode 404 (niet gevonden) worden alleen verwacht wanneer het bestand is verwijderd. Houd er rekening mee dat wanneer een bestand in een SMB verwijderen status in behandeling is, deze niet worden opgenomen in de resultaten van de lijst met bestanden. Merk ook op dat de bewerkingen die REST verwijderen bestands- en REST verwijderen atomisch doorgevoerd zijn en niet in een status in afwachting van verwijdering resulteren.  

Zie voor meer informatie:  

* [Hiermee vergrendelt u bestand beheren](https://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Samenvatting en de volgende stappen
De Microsoft Azure Storage-service is ontworpen om te voldoen aan de behoeften van de meest complexe online toepassingen zonder ontwikkelaars of denken over belangrijke veronderstellingen zoals gelijktijdigheid en consistentie van de gegevens die ze zijn gaan om te maken met verleend.  

Voor de volledige voorbeeldtoepassing waarnaar wordt verwezen in deze blog:  

* [Met behulp van Azure Storage - voorbeeldtoepassing gelijktijdigheid beheren](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Voor meer informatie over Azure Storage-Zie:  

* [Startpagina van Microsoft Azure Storage](https://azure.microsoft.com/services/storage/)
* [Kennismaking met Azure Storage](storage-introduction.md)
* Aan de slag voor opslag [Blob](../blobs/storage-dotnet-how-to-use-blobs.md), [tabel](../../cosmos-db/table-storage-how-to-use-dotnet.md), [wachtrijen](../storage-dotnet-how-to-use-queues.md), en [bestanden](../storage-dotnet-how-to-use-files.md)
* Opslagarchitectuur – [Azure Storage: een maximaal beschikbare Cloudopslagservice met sterke consistentie](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

