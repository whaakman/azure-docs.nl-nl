---
title: Beheren van Media Services-entiteiten met REST | Microsoft Docs
description: Informatie over het beheren van Media Services-entiteiten met REST-API.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 95262a32-0f2a-4286-b9e2-1a1ca6399b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: ffbf30f2bfdf0a175513a8d2b9182b35c39f6aae
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58292444"
---
# <a name="managing-media-services-entities-with-rest"></a>Beheren van Media Services-entiteiten met REST  

> [!div class="op_single_selector"]
> * [REST](media-services-rest-manage-entities.md)
> * [.NET](media-services-dotnet-manage-entities.md)
> 
> 

Microsoft Azure Media Services is een op REST gebaseerde service die is gebouwd op OData v3. U kunt toevoegen, query, bijwerken en verwijderen entiteiten in veel dezelfde manier kunt u op een andere OData-service. Uitzonderingen genoemd indien van toepassing. Zie voor meer informatie over OData [Open Data Protocol documentatie](https://www.odata.org/documentation/).

Dit onderwerp ziet u hoe u Azure Media Services entiteiten beheren met REST.

>[!NOTE]
> Vanaf 1 april 2017 wordt elke taakrecord in uw account die ouder is dan 90 dagen, automatisch verwijderd, samen met de bijbehorende onderliggende taakrecords, zelfs als het totale aantal records lager is dan het maximale quotum. Bijvoorbeeld op 1 April 2017 wordt worden elke taakrecord in uw account die ouder zijn dan 31 December 2016, automatisch verwijderd. Als u nodig hebt voor het archiveren van de taak gegevens, kunt u de code die in dit onderwerp beschreven.

## <a name="considerations"></a>Overwegingen  

Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="adding-entities"></a>Entiteiten toevoegen
Elke entiteit in Media Services wordt toegevoegd aan een entiteitsset, zoals activa, via een HTTP POST-aanvraag.

Het volgende voorbeeld ziet hoe u een AccessPolicy maakt.

    POST https://media.windows.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

## <a name="querying-entities"></a>Een query uitvoeren op entiteiten
Query's en diensten aanbieden is eenvoudig en alleen bestaat uit een GET HTTP-aanvraag en optionele OData-bewerkingen.
Het volgende voorbeeld wordt een lijst met alle MediaProcessor entiteiten.

    GET https://media.windows.net/API/MediaProcessors HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

U kunt ook een bepaalde entiteit of alle entiteitsets die zijn gekoppeld aan een bepaalde entiteit, zoals in de volgende voorbeelden ophalen:

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c')/TaskTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Het volgende voorbeeld retourneert alleen de eigenschap State van alle taken.

    GET https://media.windows.net/API/Jobs?$select=State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

Het volgende voorbeeld retourneert alle JobTemplates met de naam "SampleTemplate."

    GET https://media.windows.net/API/JobTemplates?$filter=startswith(Name,%20'SampleTemplate') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net

> [!NOTE]
> De $expand bewerking wordt niet ondersteund in Media Services, evenals de niet-ondersteunde LINQ-methoden die worden beschreven in het LINQ-overwegingen (WCF Data Services).
> 
> 

## <a name="enumerating-through-large-collections-of-entities"></a>Tot en met grote verzamelingen entiteiten opsommen
Bij het opvragen van entiteiten, is er een limiet van 1000 entiteiten in één keer wordt geretourneerd omdat openbare REST v2 queryresultaten tot 1000 resultaten beperkt. Gebruik **overslaan** en **boven** om te inventariseren door het grote aantal entiteiten. 

Het volgende voorbeeld ziet u hoe u **overslaan** en **boven** overslaan van de eerste 2000 taken en de volgende 1000 taken ophalen.  

    GET https://media.windows.net/api/Jobs()?$skip=2000&$top=1000 HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net

## <a name="updating-entities"></a>Entiteiten bijwerken
Afhankelijk van het entiteitstype en de status die deel uitmaakt, kunt u eigenschappen van die entiteit via een PATCH bijwerken PUT- of samenvoegen HTTP-aanvragen. Zie voor meer informatie over deze bewerkingen, [PATCH/PUT/MERGE](https://msdn.microsoft.com/library/dd541276.aspx).

Het volgende codevoorbeeld toont het bijwerken van de eigenschap Name op een entiteit Asset.

    MERGE https://media.windows.net/API/Assets('nb:cid:UUID:80782407-3f87-4e60-a43e-5e4454232f60') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 21
    Expect: 100-continue

    {"Name" : "NewName" }

## <a name="deleting-entities"></a>Entiteiten verwijderen
Entiteiten kunnen worden verwijderd in Media Services met behulp van een verwijderen HTTP-aanvraag. Afhankelijk van de entiteit zijn de volgorde waarin u entiteiten verwijderen belangrijk. Bijvoorbeeld: entiteiten zoals activa vereisen dat u intrekken (of verwijderen) alle Locators die verwijzen naar die bepaalde Asset voordat u verwijdert de Asset.

Het volgende voorbeeld ziet hoe u een Locator die is gebruikt voor het uploaden van een bestand naar blobopslag te verwijderen.

    DELETE https://media.windows.net/API/Locators('nb:lid:UUID:76dcc8e8-4230-463d-97b0-ce25c41b5c8d') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: media.windows.net
    Content-Length: 0

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

