---
title: Een upgrade naar de Azure Search .NET SDK versie 5 | Microsoft Docs
description: Een upgrade naar de Azure Search .NET SDK versie 5
author: brjohnstmsft
manager: jlembicz
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: brjohnst
ms.openlocfilehash: 88949f4cf0e4408f5d1e4d9c9a5833b041b5a5ab
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-5"></a>Een upgrade naar de Azure Search .NET SDK versie 5
Als u versie 4.0 preview of oudere van de [Azure Search .NET SDK](https://aka.ms/search-sdk), dit artikel helpt u bij het bijwerken van uw toepassing versie 5 te gebruiken.

Zie voor een meer algemene overzicht van de SDK, inclusief voorbeelden [het gebruik van Azure Search vanuit een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 5 van de Azure Search .NET SDK bevat een aantal wijzigingen uit eerdere versies. Dit zijn vooral kleine, zodat u uw code wijzigt, moet alleen minimale inspanning nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe versie van de SDK.

> [!NOTE]
> Als u versie 2.0 preview of ouder gebruikt, moet u eerst bijwerken naar versie 3 en vervolgens een upgrade naar versie 5. Zie [upgraden naar Azure Search .NET SDK versie 3](search-dotnet-sdk-migration.md) voor instructies.
>
> Uw Azure Search-service-exemplaar ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt blijven gebruiken van een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK, bepaalt de versie van de SDK die u in de desbetreffende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven code uit te voeren die zonder wijzigingen zelfs als de service is bijgewerkt zodat een nieuwere versie van de API wordt ondersteund.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Wat is er nieuw in versie 5
Versie 5 van de Azure Search .NET SDK gericht is op de meest recente algemeen beschikbaar versie van de Azure Search REST API, speciaal 2017-11-11. Dit maakt het mogelijk om met nieuwe functies van Azure Search vanuit een .NET-toepassing, waaronder de volgende:

* [Synoniemen](search-synonyms.md).
* U kunt nu programmatisch toegang biedt tot waarschuwingen in de geschiedenis van indexeerfunctie worden uitgevoerd (Zie de `Warning` eigenschap van `IndexerExecutionResult` in de [.NET verwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) voor meer informatie).
* Ondersteuning voor .NET Core 2.
* Nieuwe pakketstructuur ondersteunt het gebruik van alleen de delen van de SDK die u nodig hebt (Zie [wijzigingen op te splitsen in versie 5](#ListOfChanges) voor meer informatie).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het upgraden
Eerst uw NuGet-verwijzing voor bijwerken `Microsoft.Azure.Search` via de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Zodra de NuGet heeft de nieuwe pakketten en afhankelijkheden zijn gedownload, opnieuw worden opgebouwd uw project. Tenzij u een preview-functie die niet in de nieuwe GA SDK, deze moet opnieuw samenstellen heeft (zo niet, laat ons weten op [GitHub](https://github.com/azure/azure-sdk-for-net/issues)). Zo ja, bent u nu klaar voor!

Houd er rekening mee dat als gevolg van wijzigingen in het inpakken van de Azure Search .NET SDK, moet u uw toepassing om te gebruiken versie 5 opnieuw opbouwen. Deze wijzigingen worden beschreven in [wijzigingen op te splitsen in versie 5](#ListOfChanges).

Mogelijk ziet u extra build-waarschuwingen verouderd methoden of eigenschappen. De waarschuwingen bevat instructies over wat u moet gebruiken in plaats van de afgeschafte functies. Als uw toepassing gebruikt bijvoorbeeld de `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` methode, krijgt u een waarschuwing dat "Dit gedrag is nu standaard ingeschakeld, zodat u deze methode aanroept, niet meer nodig is."

Als u eventuele waarschuwingen build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 5](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Wijzigingen op te splitsen in versie 5
De meest belangrijke recente wijziging in versie 5 is dat de `Microsoft.Azure.Search` assembly en de inhoud ervan hebt is onderverdeeld in vier afzonderlijke assembly's die nu als vier afzonderlijke NuGet-pakketten worden gedistribueerd:

 - `Microsoft.Azure.Search`: Dit is een meta-pakket met alle andere Azure Search pakketten als afhankelijkheden. Als u een vanaf een eerdere versie van de SDK upgrade uitvoert moeten gewoon dit pakket upgraden en opnieuw opbouwen voldoende aan de slag met de nieuwe versie.
 - `Microsoft.Azure.Search.Data`: Dit pakket gebruiken als u een Azure Search met .NET-toepassing ontwikkelt en u alleen hoeft een query of bijwerken van documenten in uw indexen. Als u ook moet maken of bijwerken van indexen, synoniem maps of andere bronnen serviceniveau gebruiken de `Microsoft.Azure.Search` in plaats daarvan het pakket.
 - `Microsoft.Azure.Search.Service`: Dit pakket gebruiken als u automatiseren met behulp van .NET ontwikkelt voor het beheren van Azure Search-index, synoniem maps, indexeerfuncties, gegevensbronnen of andere bronnen serviceniveau. Als u alleen query of update-documenten in uw indexen moet, gebruikt u de `Microsoft.Azure.Search.Data` in plaats daarvan het pakket. Als u alle functionaliteit van Azure Search moet, gebruikt de `Microsoft.Azure.Search` in plaats daarvan het pakket.
 - `Microsoft.Azure.Search.Common`: Algemene typen die nodig is voor de Azure Search .NET-bibliotheken. U hoeft geen gebruik van dit pakket rechtstreeks in uw toepassing. Dit is alleen bedoeld als een afhankelijkheid moet worden gebruikt.
 
Deze wijziging overtreedt technisch omdat er vele typen tussen de assembly's verplaatst zijn. Daarom opnieuw opbouwen van uw toepassing is nodig om te upgraden naar versie 5 van de SDK.

Er is een klein aantal andere grote wijzigingen in versie 5 waarvoor code wijzigingen naast opnieuw opbouwen van uw toepassing.

### <a name="removed-obsolete-members"></a>Verouderd leden verwijderd

Mogelijk ziet u fouten met betrekking tot methoden of eigenschappen die zijn gemarkeerd als verouderd in eerdere versies en daarna zijn verwijderd in versie 5 bouwen. Als u dergelijke fouten optreden, wordt hier hoe u deze op te lossen:

- Als u de `IndexingParametersExtensions.IndexStorageMetadataOnly` methode gebruik `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` in plaats daarvan.
- Als u de `IndexingParametersExtensions.SkipContent` methode gebruik `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` in plaats daarvan.

### <a name="removed-preview-features"></a>Verwijderde preview-functies

Let als bij een van versie 4.0 preview-versie 5 upgrade of JSON-matrix en bij het parseren van ondersteuning voor Blob indexeerfuncties CSV is verwijderd omdat deze functies zijn nog steeds in preview. In het bijzonder de volgende methoden van de `IndexingParametersExtensions` klasse zijn verwijderd:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Als uw toepassing een vaste afhankelijkheid van deze functies heeft, kunt u zich niet bijwerken naar versie 5 van de Azure Search .NET SDK. U kunt blijven gebruiken preview-versie 4.0. Echter, neem Houd rekening met het **we raden niet met behulp van voorbeeld SDK's in productietoepassingen**. Preview-functies voor evaluatie alleen zijn en kunnen worden gewijzigd.

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van Azure Search .NET SDK nodig hebt, raadpleegt u de [.NET How-to](search-howto-dotnet-sdk.md).

Uw feedback is Welkom op de SDK. Als u problemen ondervindt, gerust ons om hulp te vragen op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat het voorvoegsel van de titel van uw probleem met '[Azure Search]'.

Hartelijk dank voor het gebruik van Azure Search.
