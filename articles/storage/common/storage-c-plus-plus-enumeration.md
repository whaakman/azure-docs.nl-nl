---
title: Lijst met Azure Storage-resources met de Opslagclientbibliotheek voor C++ | Microsoft Docs
description: Informatie over het gebruik van de aanbieding API's in Microsoft Azure Storage-clientbibliotheek voor C++ opsommen containers, blobs, wachtrijen, tabellen en entiteiten.
documentationcenter: .net
services: storage
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: 33563639-2945-4567-9254-bc4a7e80698f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.openlocfilehash: 9844412739f4f6f95416f81347f0f2eeeca62bea
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="list-azure-storage-resources-in-c"></a>Lijst van Azure Storage-resources in C++
Aanbieding-bewerkingen zijn sleutel naar veel ontwikkelscenario's met Azure Storage. Dit artikel wordt beschreven hoe u zo efficiënt mogelijk opsommen objecten in Azure Storage met behulp van de aanbieding beschikbaar in de Microsoft Azure Storage-clientbibliotheek voor C++ API's.

> [!NOTE]
> Deze handleiding is bedoeld voor de Azure Storage-clientbibliotheek voor C++ versie 2.x die beschikbaar is via [NuGet](http://www.nuget.org/packages/wastorage) of [GitHub](https://github.com/Azure/azure-storage-cpp).
> 
> 

De Opslagclientbibliotheek biedt diverse methoden om de lijst of query-objecten in Azure Storage. In dit artikel worden de volgende scenario's:

* Lijst containers in een account
* Lijst met blobs in een container of blob virtuele map
* Lijst met wachtrijen in een account
* Lijst met tabellen in een account
* Query-entiteiten in een tabel

Elk van deze methoden wordt weergegeven als u verschillende overbelastingen voor verschillende scenario's.

## <a name="asynchronous-versus-synchronous"></a>Asynchrone en synchrone
Omdat de Opslagclientbibliotheek voor C++ is gebouwd boven de [REST C++-bibliotheek](https://github.com/Microsoft/cpprestsdk), we inherent bieden ondersteuning voor asynchrone bewerkingen met behulp van [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Bijvoorbeeld:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Synchrone bewerkingen teruglopen de corresponderende asynchrone bewerkingen:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Als u met meerdere threads toepassingen of services werkt, wordt u aangeraden dat u het async-API's rechtstreeks in plaats van het maken van een thread om aan te roepen de synchronisatie-API's, die aanzienlijk heeft impact op de prestaties van uw.

## <a name="segmented-listing"></a>Gesegmenteerde aanbieding
De schaal van cloud-opslag vereist gesegmenteerde aanbieding. U kunt bijvoorbeeld via een miljoen blobs in een Azure blob-container of via een miljard entiteiten in een Azure-tabel hebben. Dit zijn geen theoretische cijfers, maar de informatie over het gebruik van echte klantaanvragen.

Daarom is het niet praktisch voor een lijst met alle objecten in een enkel antwoord. In plaats daarvan kunt u objecten met behulp van paginering aanbieden. Elk van de aanbieding API's heeft een *gesegmenteerde* overbelasting.

Het antwoord voor het uitvoeren van een gesegmenteerde aanbieding omvat:

* <i>_Segment</i>, die de verzameling resultaten geretourneerd voor één aanroep van de aanbieding API bevat.
* *continuation_token*, die wordt doorgegeven aan de volgende oproep om op te halen van de volgende pagina van de resultaten. Wanneer er geen verdere resultaten te retourneren, is het vervolgtoken is null.

Een typische aanroep voor een lijst met alle blobs in een container kan bijvoorbeeld het volgende codefragment eruit. De code is beschikbaar in onze [voorbeelden](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

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
        process_diretory(it->as_directory());
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

Als u niet geeft de *max_results* parameter, worden de standaardinstellingen in één pagina maximumwaarde van maximaal 5.000 resultaten geretourneerd.

Let ook op dat een query op Azure Table storage geen records, of minder dan de waarde van retourneren kan de *max_results* parameter die u hebt opgegeven, zelfs als het vervolgtoken niet leeg is. Een reden hiervoor is mogelijk dat de query kan niet worden voltooid in de vijf seconden. De query moet worden voortgezet, zolang het vervolgtoken niet leeg is, en uw code mogen niet de grootte van het resultaat van een segment aannemen.

Het aanbevolen codering patroon voor de meeste scenario is onderverdeeld op te nemen, waarmee u expliciete voortgang van de aanbieding of uitvoeren van query's en hoe de service reageert op elke aanvraag. Met name voor C++-toepassingen of services kunt lager niveau controle over de voortgang van de aanbieding besturingselement geheugen en de prestaties.

## <a name="greedy-listing"></a>Greedy aanbieding
Eerdere versies van de Opslagclientbibliotheek voor C++ (Preview-versies 0.5.0 en eerder) opgenomen van niet-gesegmenteerde aanbieding API's voor tabellen en wachtrijen, zoals in het volgende voorbeeld:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Deze methoden zijn geïmplementeerd als wrappers gesegmenteerde API's. Voor elke reactie van gesegmenteerde aanbieding, worden de code de resultaten toegevoegd aan een vector en alle resultaten geretourneerd nadat de volledige-containers zijn gescand.

Deze methode werkt mogelijk wanneer het opslagaccount of de tabel een klein aantal objecten bevat. Echter, met een toename in het aantal objecten, het geheugen dat nodig kan vergroten zonder beperking, omdat alle resultaten bleef in het geheugen. Één aanbieding bewerking kan erg lang duren, gedurende welke de aanroeper geen informatie over de voortgang heeft.

Deze greedy aanbieding API's in de SDK niet bestaan in C#, Java of de JavaScript-Node.js-omgeving. Om te voorkomen de potentiële problemen van het gebruik van deze greedy API's, wordt deze verwijderd in versie 0.6.0 Preview.

Als uw code deze greedy API's aanroept:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Wijzig vervolgens de code voor het gebruik van de gesegmenteerde aanbieding API's:

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

Door op te geven de *max_results* parameter van het segment kan verdeeld tussen het aantal aanvragen en geheugengebruik om te voldoen aan de prestatie-overwegingen voor uw toepassing.

Bovendien, als u gesegmenteerde aanbieding API's gebruikt, maar de gegevens opslaan in een lokale verzameling in een style 'greedy', wordt ook aangeraden dat u uw code voor het opslaan van gegevens in een lokale verzameling zorgvuldig op grote schaal afhandelen opsplitsen.

## <a name="lazy-listing"></a>Vertraagde aanbieding
Hoewel greedy aanbieding potentiële problemen gegenereerd, is het handig als er niet te veel objecten in de container.

Als u ook C# of Oracle Java SDK's gebruikt, kunt u moet bekend bent met het inventariseerbare programming model een vertraagde--stijl op te nemen biedt, waar de gegevens op een bepaalde offset alleen worden opgehaald als dat nodig is. In C++ kunt biedt de sjabloon op basis van een iterator ook een soortgelijke benadering.

Een typische vertraagde aanbieding API, met behulp van **list_blobs** bijvoorbeeld zo uitziet:

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

Houd er rekening mee vertraagde aanbieding is alleen beschikbaar in synchrone modus.

Vergeleken met greedy aanbieding, haalt vertraagde aanbieding gegevens alleen indien nodig. Onder de behandelt ophaalt het gegevens uit Azure Storage alleen wanneer de volgende iterator is verplaatst naar de volgende segment. Daarom geheugengebruik wordt beheerd met een begrensde grootte en de bewerking is snel.

Vertraagde aanbieding API's worden opgenomen in de Opslagclientbibliotheek voor C++ in versie 2.2.0.

## <a name="conclusion"></a>Conclusie
In dit artikel wordt besproken verschillende overbelastingen voor het aanbieden van API's voor verschillende objecten in de Opslagclientbibliotheek voor C++. Samengevat:

* Asynchrone APIs ten sterkste aangeraden onder scenario's met meerdere threads.
* Gesegmenteerde aanbieding wordt aanbevolen voor de meeste scenario's.
* Vertraagde aanbieding is opgegeven in de bibliotheek als een handige wrapper in synchrone scenario's.
* Greedy aanbieding wordt niet aanbevolen en is verwijderd uit de bibliotheek.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie over Azure Storage en -clientbibliotheek voor C++.

* [Het Blob Storage gebruiken met C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Hoe Table Storage gebruiken met C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Hoe Queue Storage gebruiken met C++](../storage-c-plus-plus-how-to-use-queues.md)
* [Azure Storage-clientbibliotheek voor C++ API-documentatie.](http://azure.github.io/azure-storage-cpp/)
* [Blog van het Azure Storage-team](http://blogs.msdn.com/b/windowsazurestorage/)
* [Documentatie bij Azure Storage](https://azure.microsoft.com/documentation/services/storage/)

