---
title: Lijst met Azure Storage-resources met de Storage-clientbibliotheek voor C++ | Microsoft Docs
description: Informatie over het gebruik van de aanbieding API's in Microsoft Azure Storage-clientbibliotheek voor C++ wilt inventariseren containers, blobs, wachtrijen, tabellen en entiteiten.
services: storage
author: dineshmurthy
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.component: common
ms.openlocfilehash: 2f9c7e791bf77c2387dbe9ba58b3ef70abe8fb89
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411265"
---
# <a name="list-azure-storage-resources-in-c"></a>Azure Storage-resources in C++ weergeven
Bewerkingen voor de aanbieding zijn sleutel te veel ontwikkelscenario's met Azure Storage. In dit artikel wordt beschreven hoe u zo efficiënt mogelijk het inventariseren van objecten in Azure Storage met behulp van de aanbieding beschikbaar in de Microsoft Azure Storage-clientbibliotheek voor C++ API's.

> [!NOTE]
> Deze handleiding is gericht op de Azure Storage-clientbibliotheek voor C++ versie 2.x, die beschikbaar is via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

De Opslagclientbibliotheek biedt verschillende methoden om de lijst of query objecten in Azure Storage. In dit artikel komen de volgende scenario's:

* Lijst met containers in een account
* Lijst met blobs in een container of blob virtuele map
* Lijst met wachtrijen in een account
* Lijst met tabellen in een account
* Query-entiteiten in een tabel

Elk van deze methoden wordt weergegeven met behulp van verschillende overloads voor verschillende scenario's.

## <a name="asynchronous-versus-synchronous"></a>Asynchrone en synchrone
Omdat de Storage-clientbibliotheek voor C++ is gebouwd boven de [C++ REST bibliotheek](https://github.com/Microsoft/cpprestsdk), we inherent asynchrone bewerkingen ondersteunen met behulp van [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Bijvoorbeeld:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Synchrone bewerkingen inpakken de overeenkomende asynchrone bewerkingen:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Als u met meerdere threads toepassingen of services werkt, wordt u aangeraden dat u het async-API's rechtstreeks in plaats van het maken van een thread wilt gebruiken voor het aanroepen van de synchronisatie-API's, die aanzienlijk heeft gevolgen voor de prestaties van uw.

## <a name="segmented-listing"></a>Gesegmenteerde aanbieding
De schaal van opslag in de cloud vereist gesegmenteerde aanbieding. U kunt bijvoorbeeld via een miljoen blobs in een Azure blob-container of een miljard entiteiten in een Azure-tabel hebben. Dit zijn geen theoretische getallen, maar bestaande klanten gebruik gevallen.

Daarom is het niet praktisch is om alle objecten in één antwoord weer te geven. In plaats daarvan kunt u objecten met behulp van wisselbestand weergeven. Elk van de API's van de aanbieding heeft een *gesegmenteerd* overbelasten.

Het antwoord voor de bewerking van een gesegmenteerde aanbieding bevat:

* <i>_Segment</i>, die de set resultaten geretourneerd voor een eenmalige aanroep naar de API van de aanbieding bevat.
* *continuation_token*, die wordt doorgegeven aan de volgende aanroep om op te halen van de volgende pagina van de resultaten. Als er geen verdere resultaten moeten worden geretourneerd, wordt het vervolgtoken null is.

Bijvoorbeeld, kan een typische aanroep van alle blobs in een container te vermelden lijken op het volgende codefragment. De code is beschikbaar in onze [voorbeelden](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

```cpp
// List blobs in the blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

Houd er rekening mee dat het aantal resultaten in een pagina kan worden beheerd door de parameter *max_results* in de overbelasting van elke API, bijvoorbeeld:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

Als u geen opgeeft de *max_results* parameter, de standaard maximale waarde van maximaal 5.000 resultaten wordt geretourneerd in één pagina.

Let ook op dat een query op Azure Table storage geen records, of minder dan de waarde van retourneren kan de *max_results* parameter die u hebt opgegeven, zelfs als het vervolgtoken niet leeg zijn is. Eén reden kan zijn dat de query kan niet worden voltooid in de vijf seconden. Zolang het vervolgtoken niet leeg is, de query moet blijven en uw code moet niet ervan uitgaan dat de grootte van het resultaat van een segment.

De aanbevolen codering patroon voor de meeste scenario's wordt gesegmenteerd worden vermeld, waarmee u expliciete voortgang van de aanbieding of uitvoeren van query's en hoe de service reageert op elke aanvraag. Met name voor C++-toepassingen of services kunt op lager niveau controle over de voortgang van de aanbieding besturingselement geheugen en prestaties.

## <a name="greedy-listing"></a>Greedy aanbieding
Eerdere versies van de Storage-clientbibliotheek voor C++ (Preview-versie 0.5.0 en eerder) opgenomen van niet-gesegmenteerde aanbieding API's voor tabellen en wachtrijen, zoals in het volgende voorbeeld:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Deze methoden zijn geïmplementeerd als wrappers gesegmenteerde API's. Voor elke reactie van gesegmenteerde aanbieding, wordt de code de resultaten toegevoegd aan een vector en alle resultaten geretourneerd nadat de volledige containers zijn gescand.

Deze benadering werkt mogelijk wanneer het storage-account of de tabel een klein aantal objecten bevat. Echter met een toename van het aantal objecten, kan het geheugen dat nodig verhogen zonder beperking, omdat alle resultaten in het geheugen gebleven. Een aanbieding-bewerking kan al lange tijd gedurende welke de aanroeper geen informatie over de voortgang heeft duren.

Deze greedy aanbieding API's in de SDK niet aanwezig zijn in C#, Java of de JavaScript-Node.js-omgeving. Om te voorkomen dat de potentiële problemen van het gebruik van deze greedy API's, wordt deze verwijderd in versie 0.6.0 Preview-versie.

Als uw code deze greedy API's roept:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Vervolgens wijzigt u de code voor het gebruik van de gesegmenteerde aanbieding API's:

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

Door op te geven de *max_results* parameter van het segment, u kunt in balans brengen tussen het aantal aanvragen en het geheugengebruik om te voldoen aan de prestatie-overwegingen voor uw toepassing.

Bovendien, als u gesegmenteerde aanbieding API's gebruikt, maar de gegevens opslaan in een lokale verzameling in een 'greedy' stijl, ook wordt aangeraden dat u uw code voor het afhandelen van het opslaan van gegevens in een lokale verzameling zorgvuldig op schaal herstructureren.

## <a name="lazy-listing"></a>Vertraagde aanbieding
Hoewel greedy aanbieding gegenereerd potentiële problemen, is het handig als er niet te veel objecten in de container.

Als u ook van C# of Oracle Java-SDK's gebruikmaakt, moet u bekend bent met het Invoeroverzicht programming model, deze een vertraagde--stijl worden vermeld biedt, waar de gegevens op een bepaalde offset alleen worden opgehaald als dit vereist is zijn. In C++ kunt biedt de sjabloon op basis van een iterator ook een soortgelijke benadering.

Een typische vertraagde aanbieding-API met behulp van **list_blobs** bijvoorbeeld ziet er als volgt:

```cpp
list_blob_item_iterator list_blobs() const;
```

Een typische codefragment die gebruikmaakt van het patroon vertraagde aanbieding uitzien als volgt:

```cpp
// List blobs in the blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

Houd er rekening mee dat vertraagde aanbieding alleen beschikbaar in synchrone modus is.

Vergeleken met greedy aanbieding, haalt vertraagde aanbieding gegevens alleen indien nodig. Op de achtergrond, worden deze gegevens opgehaald uit Azure Storage alleen wanneer de volgende iterator is verplaatst naar de volgende segment. Daarom geheugengebruik wordt beheerd met een begrensde grootte en de bewerking is snel.

Vertraagde aanbieding API's zijn opgenomen in de Storage-clientbibliotheek voor C++ in versie 2.2.0.

## <a name="conclusion"></a>Conclusie
In dit artikel wordt besproken verschillende overloads voor het aanbieden van API's voor verschillende objecten in de Storage-clientbibliotheek voor C++. Samenvatting:

* Asynchrone APIs zijn sterk aanbevolen in scenario's met meerdere threads.
* Gesegmenteerde aanbieding wordt aanbevolen voor de meeste scenario's.
* Vertraagde aanbieding is beschikbaar in de bibliotheek als een handige wrapper in synchrone scenario's.
* Greedy aanbieding wordt niet aanbevolen en is verwijderd uit de bibliotheek.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en -clientbibliotheek voor C++.

* [Het Blob Storage gebruiken met C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hoe u kunt Table Storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Hoe u Queue Storage gebruiken met C++](../storage-c-plus-plus-how-to-use-queues.md)
* [Azure Storage-clientbibliotheek voor C++ API-documentatie.](http://azure.github.io/azure-storage-cpp/)
* [Blog van het Azure Storage-team](https://blogs.msdn.com/b/windowsazurestorage/)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)

