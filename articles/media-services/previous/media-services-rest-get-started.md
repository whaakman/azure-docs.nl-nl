---
title: Aan de slag met het leveren van inhoud op aanvraag met behulp van REST | Microsoft Docs
description: Deze zelfstudie leert u de stappen van de implementatie van een on demand levering van inhoud-toepassing met Azure Media Services met behulp van REST-API.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 88194b59-e479-43ac-b179-af4f295e3780
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 0efbabf658210c733a7a7f201cb4a36f63456b28
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57835340"
---
# <a name="get-started-with-delivering-content-on-demand-using-rest"></a>Aan de slag met het leveren van inhoud op aanvraag met behulp van REST  

[!INCLUDE [media-services-selector-get-started](../../../includes/media-services-selector-get-started.md)]

In deze snelstartgids leidt u door de stappen van de implementatie van een video on demand (VoD) contentlevering-toepassing met behulp van Azure Media Services (AMS) REST-API's.

In deze zelfstudie maakt u kennis met de algemene werkstroom voor Media Services en de meest algemene programmeerobjecten en -taken die zijn vereist voor het ontwikkelen van Media Services. Na het voltooien van de zelfstudie bent u streamen of progressief downloaden van een voorbeeldbestand van de media die u geüpload, gecodeerd en gedownload.

In de volgende afbeelding ziet u een aantal van de meest gebruikte objecten bij het ontwikkelen van VoD-toepassingen in het Media Services OData-model.

Klik op de afbeelding om deze in volledig formaat weer te geven.  

<a href="./media/media-services-rest-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-rest-get-started/media-services-overview-object-model-small.png"></a> 

## <a name="prerequisites"></a>Vereisten
De volgende vereisten zijn vereist om te beginnen met het ontwikkelen met Media Services met REST-API's.

* Een Azure-account. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie.
* Een Media Services-account. Zie [Een Media Services-account maken](media-services-portal-create-account.md) voor meer informatie over het maken van een Media Services-account.
* Inzicht in hoe u voor het ontwikkelen met Media Services REST API. Zie voor meer informatie, [Media Services REST API-overzicht](media-services-rest-how-to-use.md).
* Een toepassing van uw keuze die HTTP-aanvragen en antwoorden kan verzenden. Deze zelfstudie wordt gebruikgemaakt van [Fiddler](https://www.telerik.com/download/fiddler).

De volgende taken worden weergegeven in deze Quick Start.

1. Streaming-eindpunt starten (vanuit Azure Portal).
2. Verbinding maken met het Media Services-account met REST-API.
3. Een nieuwe asset maken en een video uploaden met REST-API.
4. Het bronbestand coderen in een set adaptive bitrate MP4-bestanden met REST-API.
5. De asset publiceren en streamen en progressief downloaden ophalen URL's met REST-API.
6. Uw inhoud afspelen.

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). Gebruik dezelfde beleids-ID als u altijd dezelfde dagen / toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om te blijven aanwezig gedurende een lange periode (niet-uploadbeleidsregels). Raadpleeg [dit artikel](media-services-dotnet-manage-entities.md#limit-access-policies) voor meer informatie.

Zie voor meer informatie over de AMS-REST-entiteiten die worden gebruikt in dit artikel [Azure Media Services REST API-verwijzing](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference). Zie ook [Azure Media Services-concepten](media-services-concepts.md).

>[!NOTE]
>Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="start-streaming-endpoints-using-the-azure-portal"></a>Streaming-eindpunten starten met behulp van Azure Portal

Als u werkt met Azure Media Services, wordt een van de meest voorkomende scenario's video via adaptive bitratestreaming geleverd. Media Services biedt dynamische pakketten waarmee u uw Adaptive Bitrate MP4-inhoud 'just in time' kunt leveren in de streaming-indelingen die door Media Services worden ondersteund (MPEG DASH, HLS, Smooth Streaming), zonder dat u vooraf verpakte versies van elk van deze streaming-indelingen hoeft op te slaan.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben.

U start het streaming-eindpunt als volgt:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2. Klik in het venster Instellingen op Streaming-eindpunten.
3. Klik op het standaardstreaming-eindpunt.

    Het venster DETAILS VAN STANDAARDSTREAMING-EINDPUNT wordt weergegeven.

4. Klik op het pictogram Start.
5. Klik op de knop Opslaan om uw wijzigingen op te slaan.

## <a id="connect"></a>Verbinding maken met het Media Services-account met REST-API

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

## <a id="upload"></a>Een nieuwe asset maken en uploaden van een videobestand met REST-API

In Media Services uploadt u de digitale bestanden naar (of neemt u deze op in) een asset. De **Asset** entiteit kan bevatten, video, audio, afbeeldingen, verzamelingen van miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden in de asset zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming.

Een van de waarden die u opgeven moet bij het maken van een asset is opties voor het maken. De **opties** eigenschap is een opsommingswaarde die de versleutelingsopties dat een Asset kan worden gemaakt met beschrijft. Een geldige waarde is een van de waarden uit de onderstaande lijst, niet een combinatie van waarden uit deze lijst:

* **Geen** = **0** -geen versleuteling wordt gebruikt. Wanneer u deze optie wordt uw inhoud is niet beveiligd tijdens de overdracht of in rust in de opslag.
    Als u een MP4-bestand wilt leveren via progressief downloaden, gebruikt u deze optie.
* **StorageEncrypted** = **1** - versleutelt uw niet-versleutelde inhoud lokaal via AES-256-bits codering en vervolgens geüpload naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. De versleuteling van assets die zijn beveiligd met Storage Encryption, wordt automatisch ongedaan gemaakt en de assets worden automatisch in een versleuteld bestandssysteem geplaatst voordat ze worden gecodeerd. Eventueel kunnen ze opnieuw worden versleuteld voordat ze opnieuw worden geüpload als een nieuwe uitvoerasset. Storage Encryption wordt voornamelijk gebruikt om uw invoerbestanden met media van hoge kwaliteit die zijn opgeslagen op de schijf, te beveiligen met een sterke versleuteling.
* **CommonEncryptionProtected** = **2** -Gebruik deze optie als u inhoud uploadt die al is versleuteld en beveiligd met Common Encryption of PlayReady DRM (bijvoorbeeld Smooth Streaming beveiligd met PlayReady DRM).
* **EnvelopeEncryptionProtected** = **4** : Gebruik deze optie als u met AES versleutelde HLS uploadt. De bestanden zijn moeten gecodeerd en versleuteld door Transform Manager.

### <a name="create-an-asset"></a>Maak een asset
Een asset is een container voor meerdere typen of sets van objecten in Media Services, waaronder video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden bestanden. In de REST-API vereist het maken van een Asset POST-aanvraag verzenden naar Media Services en alle informatie over uw asset plaatsen in de aanvraagtekst.

Het volgende voorbeeld ziet hoe u een asset te maken.

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45

    {"Name":"BigBuckBunny.mp4", "Options":"0"}


**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

### <a name="create-an-assetfile"></a>Een AssetFile maken
De [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteit vertegenwoordigt een video of audio-bestand dat is opgeslagen in een blob-container. Een assetbestand is altijd gekoppeld aan een asset en een asset kan een of meer AssetFiles bevatten. De Media Services Encoder-taak mislukt als een object van het bestand asset niet gekoppeld aan een digitaal bestand in een blob-container is.

Nadat u uw digitale media-bestand naar een blob-container uploaden, gebruikt u de **samenvoegen** HTTP-aanvraag voor het bijwerken van de AssetFile met informatie over uw media-bestand (zoals later in het onderwerp).

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 164

    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Het maken van de AccessPolicy met de machtiging schrijven
Voordat u bestanden uploadt naar blob storage, stel het beleid rechten voor het schrijven naar een asset. Om dit te doen, PLAATST u een HTTP-aanvraag naar de entiteit AccessPolicies set. Een waarde DurationInMinutes bij het maken van definiëren of ontvangt u 500 Interne Server het foutbericht terug in het antwoord. Zie voor meer informatie over AccessPolicies [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

Het volgende voorbeeld laat zien hoe een AccessPolicy maken:

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74

    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"}

**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

### <a name="get-the-upload-url"></a>De Upload-URL ophalen

Voor het ontvangen van de werkelijke upload-URL, een SAS-Locator te maken. Locators definieert de begin- en type van het verbindingseindpunt voor clients die toegang tot bestanden in een Asset. U kunt meerdere Locator entiteiten voor een bepaalde AccessPolicy en Asset paar voor het afhandelen van aanvragen van andere clients en moet maken. Elk van deze Locators de StartTime-waarde plus de DurationInMinutes-waarde van de AccessPolicy gebruikt om te bepalen hoe lang die een URL kan worden gebruikt. Zie voor meer informatie, [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).

Een SAS-URL heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Hierbij geldt het volgende:

* U kunt geen meer dan vijf unieke Locators die zijn gekoppeld aan een opgegeven activum in één keer. 
* Als u nodig hebt onmiddellijk uw bestanden uploaden, moet u de waarde StartTime ingesteld op vijf minuten vóór de huidige tijd. Dit is omdat er mogelijk klok scheeftrekken tussen uw client-computer en Media Services. De waarde StartTime moet ook de volgende datum/tijd-indeling zijn: JJJJ-MM-ddTHH (bijvoorbeeld: "2014-05-23T17:53:50Z ').    
* Mogelijk zijn er een tweede 30-40 vertraging nadat een Locator is gemaakt op wanneer deze beschikbaar voor gebruik. Dit probleem is van toepassing op beide [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) en oorsprong Locators.

Het volgende voorbeeld ziet hoe u een SAS-URL-Locator maakt zoals gedefinieerd door de eigenschap Type in de hoofdtekst van de aanvraag ("1" voor een SAS-locator) en "2" voor een On-Demand origin-locator. De **pad** geretourneerd van de eigenschap bevat de URL die u gebruiken moet om uw bestand te uploaden.

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178

    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Een bestand uploaden naar een blob storage-container
Zodra u hebt de AccessPolicy en Locator ingesteld, wordt het werkelijke bestand geüpload naar een Azure blob storage-container met behulp van de Azure Storage REST-API's. U moet de bestanden uploaden als blok-blobs. Pagina-blobs worden niet ondersteund door Azure Media Services.  

> [!NOTE]
> U moet de bestandsnaam voor het bestand dat u wilt uploaden naar de Locator toevoegen **pad** waarde die is ontvangen in de vorige sectie. Bijvoorbeeld `https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4?`.
>
>

Zie voor meer informatie over het werken met Azure storage-blobs [REST API voor Blob Service](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

### <a name="update-the-assetfile"></a>De AssetFile bijwerken
Nu dat u het bestand hebt geüpload, moet u de informatie FileAsset grootte (en andere) bijwerken. Bijvoorbeeld:

    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net

    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content
    ...

## <a name="delete-the-locator-and-accesspolicy"></a>De Locator en AccessPolicy verwijderen
**HTTP-aanvraag**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content
    ...

**HTTP-aanvraag**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

    HTTP/1.1 204 No Content
    ...

## <a id="encode"></a>Het bronbestand coderen in een set adaptive bitrate MP4-bestanden

Na het opnemen van de die activa in Media Services, media kunnen worden gecodeerd, transmuxed, van een watermerk enzovoort, voordat deze aan clients wordt geleverd. Deze activiteiten worden gepland en uitgevoerd op meerdere achtergrondrolinstanties om hoge prestaties en een hoge beschikbaarheid te garanderen. Deze activiteiten worden taken genoemd en elke taak bestaat uit atomische taken die daadwerkelijk werken op het assetbestand (Zie voor meer informatie, [taak](https://docs.microsoft.com/rest/api/media/operations/job), [taak](https://docs.microsoft.com/rest/api/media/operations/task) beschrijvingen).

Zoals eerder al is aangegeven, bij het werken met Azure Media Services is een van de meest voorkomende scenario's adaptive bitrate streaming geleverd aan uw clients. Met Media Services kunt u een dynamisch pakket maken met een van de volgende indelingen voor MP4-bestanden met een adaptieve bitsnelheid: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.

De volgende sectie ziet hoe u een taak met een coderingstaak te maken. De taak bevat voor het transcoderen het tussentijdse bestand in een set met behulp van adaptieve bitrate MP4s **Media Encoder Standard**. De sectie wordt ook uitgelegd hoe u voor het bewaken van de taak voortgang verwerken. Wanneer de taak voltooid is, wordt u mogelijk zijn om locators die nodig zijn om toegang te krijgen tot uw bedrijfsmiddelen te maken.

### <a name="get-a-media-processor"></a>Een Mediaprocessor ophalen
In Media Services is een Mediaprocessor een component die verantwoordelijk is voor een specifieke verwerkingstaak, zoals codering, Indelingsconversie, versleutelen of ontsleutelen media-inhoud. Voor de coderingstaak wordt weergegeven in deze zelfstudie, gaan we de Media Encoder Standard gebruikt.

De volgende code aanvragen van het coderingsprogramma-id.

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-antwoord**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Een taak maken
Elke taak kan een of meer taken, afhankelijk van het type van de verwerking die u wilt bereiken hebben. U kunt via de REST-API, taken en hun verwante taken maken op twee manieren: Taken kunnen worden gedefinieerd inline via de navigatie-eigenschap van de taken op taak entiteiten of batchverwerking van OData. Batchverwerking maakt gebruik van de Media Services SDK. Voor de leesbaarheid van de codevoorbeelden in dit artikel zijn taken echter gedefinieerde inline. Zie voor informatie over batchverwerking, [Open Data Protocol (OData) batchverwerking](https://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Het volgende voorbeeld ziet u het maken en een taak met een die taak instellen op het coderen van een video op een specifieke oplossingsstatus en de kwaliteit te plaatsen. De volgende sectie van de documentatie bevat de lijst met alle de [taak voorinstellingen](https://msdn.microsoft.com/library/mt269960) ondersteund door de processor Media Encoder Standard.  

**HTTP-aanvraag**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"Adaptive Streaming",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  

             ]
          }
       }
    }


Er zijn een paar dingen die belangrijk te weten als u een taak doet:

* Eigenschappen van TaskBody moeten letterlijke XML gebruiken om te definiëren van het aantal invoer of uitvoer van de activa die worden gebruikt door de taak. Taak dit artikel bevat de definitie van de XML-Schema voor het XML-bestand.
* In het definitie TaskBody elke interne waarde voor de <inputAsset> en <outputAsset> JobInputAsset(value) of JobOutputAsset(value) moet worden ingesteld.
* Een taak kan meerdere uitvoerassets hebben. Een JobOutputAsset(x) kan alleen één keer worden gebruikt als uitvoer van een taak in een taak.
* U kunt JobInputAsset of JobOutputAsset opgeven als een invoeractivum van een taak.
* Taken moeten een cyclus voor besturingselementen geen cyclus.
* De waardeparameter die u aan JobInputAsset of JobOutputAsset doorgeeft vertegenwoordigt de indexwaarde voor een Asset. De werkelijke activa zijn gedefinieerd in de eigenschappen van de navigatie InputMediaAssets en OutputMediaAssets op de definitie van de taak entiteit.

> [!NOTE]
> Omdat Media Services is gebouwd op OData v3-processors, de afzonderlijke elementen in InputMediaAssets en OutputMediaAssets navigatie-eigenschap verzamelingen wordt verwezen door een ' __metadata: uri ' naam / waarde-paar.
>
>

* InputMediaAssets toegewezen aan een of meer elementen die u hebt gemaakt in Media Services. OutputMediaAssets worden gemaakt door het systeem. Ze bevatten geen verwijzing naar een bestaande asset.
* OutputMediaAssets kan de naam met behulp van het kenmerk assetName. Als dit kenmerk niet aanwezig zijn is, is de naam van de OutputMediaAsset ongeacht de binnenste tekstwaarde van de <outputAsset> -element is met een achtervoegsel van de waarde van de taak, of de taak-Id-waarde (in het geval waarbij de eigenschap Name is niet gedefinieerd). Bijvoorbeeld, als u een waarde instellen voor assetName naar "Voorbeeld", zou klikt u vervolgens de eigenschap OutputMediaAsset Name worden ingesteld op "Voorbeeld". Als u een waarde voor assetName niet is ingesteld, maar de naam van de taak naar nieuwe "taak" hebt ingesteld, zou klikt u vervolgens de naam van de OutputMediaAsset wel '_NewJob JobOutputAsset (waarde)'.

    Het volgende voorbeeld laat zien hoe het kenmerk assetName instellen:

        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"
* Om in te schakelen taak koppeling:

  * Een taak moet ten minste twee taken hebben
  * Er moet ten minste één taak waarvan invoer is de uitvoer van een andere taak in de taak.

Zie voor meer informatie, [het maken van een taak codering met Media Services REST API](media-services-rest-encode-asset.md).

### <a name="monitor-processing-progress"></a>Monitor voortgang verwerken
U kunt de taakstatus ophalen met behulp van de eigenschap State zoals wordt weergegeven in het volgende voorbeeld:

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT

    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Een taak annuleren
Media Services kunt u de taken die worden uitgevoerd via de functie CancelJob annuleren. Deze aanroep retourneert een foutcode 400 als u probeert te annuleren van een taak wanneer de status wordt geannuleerd, annuleren, fout of voltooid.

Het volgende voorbeeld ziet hoe u aan te roepen CancelJob.

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net


Als dit lukt, wordt een 204 antwoordcode wordt geretourneerd bij geen berichttekst.

> [!NOTE]
> U moet coderen met een URL van de taak-id (normaal gesproken nb:jid:UUID: deze functie) als deze wordt doorgegeven als parameter aan CancelJob.
>
>

### <a name="get-the-output-asset"></a>Ophalen van de uitvoerasset
De volgende code laat zien hoe u aan te vragen van de uitvoerasset id.

**HTTP-aanvraag**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-antwoord**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }

## <a id="publish_get_urls"></a>De asset publiceren en te streamen en progressief downloaden ophalen URL's met REST-API

Als u een asset wilt streamen of downloaden, moet u deze eerste publiceren door een locator te maken. Locators bieden toegang tot bestanden in de asset. Media Services ondersteunt twee typen locators: OnDemandOrigin-locators, voor het streamen van media (bijvoorbeeld MPEG DASH, HLS, of Smooth Streaming) en SAS-locators (Shared Access Signature), voor het downloaden van media-bestanden. 

Zodra u de locators hebt gemaakt, kunt u de URL's die worden gebruikt om te streamen of te downloaden van uw bestanden kunt bouwen.

>[!NOTE]
>Wanneer uw AMS-account is gemaakt, wordt er een **standaardstreaming-eindpunt** met de status **Gestopt** toegevoegd aan uw account. Als u inhoud wilt streamen en gebruik wilt maken van dynamische pakketten en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben.

Een streaming-URL voor Smooth Streaming heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Een streaming-URL voor HLS heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Een streaming-URL voor MPEG DASH heeft de volgende indeling:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Een SAS-URL die wordt gebruikt om bestanden te downloaden, heeft de volgende indeling:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Deze sectie beschrijft hoe u de volgende taken uitvoeren die nodig zijn voor uw activa 'publiceren'.  

* Het maken van de AccessPolicy met leesmachtiging
* Het maken van een SAS-URL voor het downloaden van inhoud
* Het maken van een oorsprong-URL voor het streamen van inhoud

### <a name="creating-the-accesspolicy-with-read-permission"></a>Het maken van de AccessPolicy met leesmachtiging
Voordat u het downloaden of een media-inhoud streamen, eerst een AccessPolicy met lees-en schrijfmachtigingen definiëren en maken van de juiste Locator-entiteit die geeft het type van een leveringsmechanisme die u wilt inschakelen voor uw clients. Zie voor meer informatie over de beschikbare eigenschappen [AccessPolicy entiteitseigenschappen](https://docs.microsoft.com/rest/api/media/operations/accesspolicy#accesspolicy_properties).

Het volgende voorbeeld ziet hoe u kunt de AccessPolicy voor lees-en schrijfmachtigingen voor een bepaalde Asset opgeven.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue

    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Als dit lukt, wordt een 201 succescode geretourneerd met een beschrijving van de AccessPolicy-entiteit die u hebt gemaakt. Vervolgens gebruikt u de AccessPolicy Id samen met de Asset-Id van de asset die het bestand dat u afleveren (zoals een uitvoerasset wilt) voor het maken van de entiteit Locator bevat.

> [!NOTE]
> Deze basiswerkstroom is hetzelfde als het uploaden van een bestand bij het opnemen van een Asset (zoals eerder in dit onderwerp is beschreven). Ook, zoals het uploaden van bestanden, als u (of uw clients) moeten onmiddellijk toegang krijgen tot uw bestanden, stelt de waarde StartTime tot vijf minuten vóór de huidige tijd. Deze actie is nodig omdat er mogelijk klok scheeftrekken tussen de client en Media Services. De waarde StartTime moet zich in de volgende datum/tijd-indeling: JJJJ-MM-ddTHH (bijvoorbeeld: "2014-05-23T17:53:50Z ').
>
>

### <a name="creating-a-sas-url-for-downloading-content"></a>Het maken van een SAS-URL voor het downloaden van inhoud
De volgende code toont hoe u een URL die kan worden gebruikt voor het downloaden van een media-bestand hebt gemaakt en eerder hebt geüpload. De AccessPolicy leesmachtigingen machtigingen zijn ingesteld en het pad Locator verwijst naar een SAS-URL voor downloaden.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }

De geretourneerde **pad** eigenschap bevat de SAS-URL.

> [!NOTE]
> Als u opslag versleuteld inhoud downloaden, moet u handmatig ontsleutelen voordat deze, of de opslag ontsleuteling MediaProcessor gebruiken in een taak voor de verwerking van verwerkte bestanden naar een OutputAsset in de uitvoer en downloaden van deze Asset. Zie voor meer informatie over de verwerking, het maken van een taak codering met Media Services REST API. Bovendien kan niet Locators voor SAS-URL worden bijgewerkt nadat ze zijn gemaakt. Bijvoorbeeld, dat u de dezelfde Locator met een bijgewerkte StartTime-waarde opnieuw kan niet gebruiken. Dit is vanwege de manier waarop de die SAS-URL's worden gemaakt. Als u toegang hebben tot een asset gedownload wilt nadat een Locator is verlopen, moet u een nieuw bestand met een nieuwe StartTime maken.
>
>

### <a name="download-files"></a>Bestanden downloaden
Zodra u hebt de AccessPolicy en Locator ingesteld, kunt u bestanden met behulp van de Azure Storage REST-API's kunt downloaden.  

> [!NOTE]
> U moet de bestandsnaam voor het bestand dat u wenst te downloaden naar de Locator toevoegen **pad** waarde die is ontvangen in de vorige sectie. Bijvoorbeeld: https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . .

Zie voor meer informatie over het werken met Azure storage-blobs [REST API voor Blob Service](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Als gevolg van de coderingstaak die u eerder (codering in adaptieve MP4-set) hebt uitgevoerd, hebt u meerdere MP4-bestanden die u kunt progressief downloaden. Bijvoorbeeld:    

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

### <a name="creating-a-streaming-url-for-streaming-content"></a>Het maken van een streaming-URL voor het streamen van inhoud
De volgende code toont hoe u een streaming-URL-Locator maakt:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.17
    Authorization: Bearer <ENCODED JWT TOKEN>
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue

    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT

    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Als u wilt een oorsprong Smooth Streaming-URL in een streaming MediaPlayer streamen, moet u het pad naar de eigenschap met de naam van de Smooth Streaming manifestbestand, gevolgd door ' / manifest ' toevoegen.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Om te streamen HLS, toevoegen (format = m3u8-aapl) nadat de '/ manifest'.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Om te streamen MPEG DASH, toevoegen (format = mpd-time-csf) nadat de '/ manifest'.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Uw inhoud afspelen
Gebruik [Azure Media Services Player](https://amsplayer.azurewebsites.net/azuremediaplayer.html) om uw video te streamen.

Als u wilt testen progressief downloaden, plakt u een URL in een browser (bijvoorbeeld Internet Explorer, Chrome, Safari).

## <a name="next-steps-media-services-learning-paths"></a>Volgende stappen Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
