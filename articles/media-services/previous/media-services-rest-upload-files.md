---
title: Bestanden uploaden naar een Azure Media Services-account met behulp van REST | Microsoft Docs
description: Informatie over het verkrijgen van media-inhoud in Media Services door te maken en uploaden van activa.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: juliako
ms.openlocfilehash: 3b5c277f51b8ff1b2d3babf23329dcde829573a9
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55813956"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Bestanden uploaden naar een Media Services-account met behulp van REST
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 

In Media Services uploadt u de digitale bestanden naar (of neemt u deze op in) een asset. De [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entiteit kan bevatten, video, audio, afbeeldingen, verzamelingen van miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden (en de metagegevens over deze bestanden.)  Zodra de bestanden in de asset zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

In deze zelfstudie leert u hoe u een bestand en andere bewerkingen die zijn gekoppeld aan het uploaden:

> [!div class="checklist"]
> * Postman instellen voor alle uploadbewerkingen
> * Verbinding met Media Services maken 
> * Een toegangsbeleid maken met de machtiging schrijven
> * Maak een asset
> * Een SAS-locator te maken en de upload-URL
> * Een bestand uploaden naar blob-opslag met de upload-URL
> * Maken van een metagegevens in de asset voor de media-bestand dat u hebt geüpload

## <a name="prerequisites"></a>Vereisten

- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [Een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
- Controleer de [toegang tot Azure Media Services API met AAD-verificatieoverzicht](media-services-use-aad-auth-to-access-ams-api.md) artikel.
- Configure **Postman** zoals beschreven in [Postman configureren voor Media Services REST API-aanroepen](media-rest-apis-with-postman.md).

## <a name="considerations"></a>Overwegingen

De volgende overwegingen zijn van toepassing wanneer u Media Services REST API:
 
* Bij het openen van entiteiten met behulp van Media Services REST API, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md). <br/>De Postman-verzameling in deze zelfstudie gebruikt zorgt dat alle benodigde headers instellen.
* Media Services wordt de waarde van de eigenschap IAssetFile.Name bij het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
* De lengte van de naam mag niet groter zijn dan 260 tekens lang zijn.
* Er is een limiet voor de maximale bestandsgrootte die wordt ondersteund voor verwerking in Media Services. Raadpleeg [dit](media-services-quotas-and-limitations.md) artikel voor meer informatie over de maximale bestandsgrootte.

## <a name="set-up-postman"></a>Postman instellen

Zie voor instructies over het instellen van Postman voor deze zelfstudie, [Postman configureren](media-rest-apis-with-postman.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

1. Verbindingswaarden toevoegen aan uw omgeving. 

    Enkele variabelen die deel van uitmaken de **MediaServices** [omgeving](postman-environment.md) moet handmatig worden ingesteld voordat u met het uitvoeren van bewerkingen die zijn gedefinieerd beginnen kunt de [verzameling](postman-collection.md).

    Als u waarden voor de eerste vijf variabelen, Zie [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-import-env.png)
2. Geef de waarde voor de **MediaFileName** omgevingsvariabele.

    Geef de bestandsnaam op van de media die u van plan bent om te uploaden. In dit voorbeeld gaan we de BigBuckBunny.mp4 uploaden. 
3. Bekijk de **AzureMediaServices.postman_environment.json** bestand. U ziet dat bijna alle bewerkingen in de verzameling een 'test'-script uitvoeren. De scripts worden enkele waarden geretourneerd door het antwoord en juiste omgevingsvariabelen worden ingesteld.

    Bijvoorbeeld, de eerste bewerking opgehaald van een toegangstoken en instellen op de **AccessToken** omgevingsvariabele die wordt gebruikt in alle andere bewerkingen.

    ```    
    "listen": "test",
    "script": {
        "type": "text/javascript",
        "exec": [
            "var json = JSON.parse(responseBody);",
            "postman.setEnvironmentVariable(\"AccessToken\", json.access_token);"
        ]
    }
    ```
4. Aan de linkerkant van de **Postman** venster, klikt u op **1. AAD-verificatie-token ophalen** -> **Azure AD Token ophalen voor de Service-Principal**.

    Het gedeelte van de URL wordt gevuld met de **AzureADSTSEndpoint** omgevingsvariabele (eerder in de zelfstudie, stelt u de waarden van omgevingsvariabelen die ondersteuning bieden voor de verzameling).

    ![Bestand uploaden](./media/media-services-rest-upload-files/postment-get-token.png)

5. Druk op **Verzenden**.

    Hier ziet u het antwoord dat 'access_token' bevat. Het script 'test' neemt deze waarde en stelt de **AccessToken** omgevingsvariabele (zoals hierboven beschreven). Als u de omgevingsvariabelen bekijkt, ziet u deze variabele bevat nu de access token (bearer-token)-waarde die wordt gebruikt in de rest van de bewerkingen. 

    Als het token verloopt de 'Ophalen Azure AD-Token voor Service Principal' stap opnieuw doorlopen. 

## <a name="create-an-access-policy-with-write-permission"></a>Een toegangsbeleid maken met de machtiging schrijven

### <a name="overview"></a>Overzicht 

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit artikel](media-services-dotnet-manage-entities.md#limit-access-policies) voor meer informatie.

Voordat u bestanden uploadt naar blob storage, stel het beleid rechten voor het schrijven naar een asset. Om dit te doen, PLAATST u een HTTP-aanvraag naar de entiteit AccessPolicies set. Een waarde DurationInMinutes bij het maken van definiëren of ontvangt u 500 Interne Server het foutbericht terug in het antwoord. Zie voor meer informatie over AccessPolicies [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

### <a name="create-an-access-policy"></a>Een toegangsbeleid maken

1. Selecteer **AccessPolicy** -> **AccessPolicy maken voor het uploaden van**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-access-policy.png)

    Het script 'test' de AccessPolicy-Id opgehaald en ingesteld van de juiste omgevingsvariabele.

## <a name="create-an-asset"></a>Maak een asset

### <a name="overview"></a>Overzicht

Een [asset](https://docs.microsoft.com/rest/api/media/operations/asset) is een container voor meerdere typen of sets van objecten in Media Services, waaronder video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden bestanden. In de REST-API vereist het maken van een Asset POST-aanvraag verzenden naar Media Services en alle informatie over uw asset plaatsen in de aanvraagtekst.

Een van de eigenschappen die u toevoegen kunt bij het maken van een asset is **opties**. U kunt een van de volgende versleutelingsopties opgeven: **Geen** (standaard geen versleuteling wordt gebruikt), **StorageEncrypted** (voor inhoud die vooraf zijn versleuteld met opslagversleuteling op client-side is), **CommonEncryptionProtected**, of  **EnvelopeEncryptionProtected**. Wanneer u een gecodeerde asset hebt, moet u een beleid voor de levering configureren. Zie voor meer informatie, [leveringsbeleid voor Assets configureren](media-services-rest-configure-asset-delivery-policy.md).

Als uw asset is versleuteld, moet u een **ContentKey** en deze koppelen aan uw activa, zoals beschreven in het volgende artikel: [Over het maken van een ContentKey](media-services-rest-create-contentkey.md). Nadat u de bestanden in de asset uploadt, moet u bijwerken van de eigenschappen van de versleuteling op de **AssetFile** entiteit met de waarden die u hebt verkregen tijdens de **Asset** versleuteling. Dit doen met behulp van de **samenvoegen** HTTP-aanvraag. 

In dit voorbeeld maken we een niet-gecodeerde asset. 

### <a name="create-an-asset"></a>Maak een asset

1. Selecteer **activa** -> **Asset maken**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-asset.png)

    Het script 'test' de Asset-Id opgehaald en ingesteld van de juiste omgevingsvariabele.

## <a name="create-a-sas-locator-and-create-the-upload-url"></a>Een SAS-locator te maken en de URL uploaden

### <a name="overview"></a>Overzicht

Zodra u hebt de AccessPolicy en Locator ingesteld, wordt het werkelijke bestand geüpload naar een Azure Blob Storage-container met behulp van de Azure Storage REST-API's. U moet de bestanden uploaden als blok-blobs. Pagina-blobs worden niet ondersteund door Azure Media Services.  

Zie voor meer informatie over het werken met Azure storage-blobs [REST API voor Blob Service](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Voor het ontvangen van de werkelijke upload-URL, maakt u een SAS-Locator (Zie hieronder). Locators definieert de begin- en type van het verbindingseindpunt voor clients die toegang tot bestanden in een Asset. U kunt meerdere Locator entiteiten voor een bepaalde AccessPolicy en Asset paar voor het afhandelen van aanvragen van andere clients en moet maken. Elk van deze Locators de StartTime-waarde plus de DurationInMinutes-waarde van de AccessPolicy gebruikt om te bepalen hoe lang die een URL kan worden gebruikt. Zie voor meer informatie, [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).

Een SAS-URL heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

### <a name="considerations"></a>Overwegingen

Hierbij geldt het volgende:

* U kunt geen meer dan vijf unieke Locators die zijn gekoppeld aan een opgegeven activum in één keer. Zie voor meer informatie, Locator.
* Als u nodig hebt onmiddellijk uw bestanden uploaden, moet u de waarde StartTime ingesteld op vijf minuten vóór de huidige tijd. Dit is omdat er mogelijk klok scheeftrekken tussen uw client-computer en Media Services. De waarde StartTime moet ook de volgende datum/tijd-indeling zijn: JJJJ-MM-ddTHH (bijvoorbeeld: "2014-05-23T17:53:50Z ').    
* Mogelijk zijn er een tweede 30-40 vertraging nadat een Locator is gemaakt op wanneer deze beschikbaar voor gebruik.

### <a name="create-a-sas-locator"></a>Een SAS-locator te maken

1. Selecteer **Locator** -> **SAS-Locator maakt**.
2. Druk op **Verzenden**.

    Het script 'test' maakt de "uploaden URL' op basis van de media-bestandsnaam die u hebt opgegeven en de SAS-locator informatie en stelt de juiste omgevingsvariabele.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-sas-locator.png)

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Een bestand uploaden naar blob-opslag met de upload-URL

### <a name="overview"></a>Overzicht

Nu dat u de upload-URL hebt, moet u het schrijven van code met behulp van de Azure Blob-API's rechtstreeks naar uw bestand uploaden naar de container SAS. Raadpleeg voor meer informatie de volgende artikelen:

- [Met behulp van de Azure Storage REST-API](https://docs.microsoft.com/azure/storage/common/storage-rest-api-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Blob plaatsen](https://docs.microsoft.com/rest/api/storageservices/put-blob)
- [Blobs uploaden naar Blob-opslag](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage)

### <a name="upload-a-file-with-postman"></a>Upload een bestand met Postman

Bijvoorbeeld: we Postman gebruiken om een kleine MP4-bestand te uploaden. Mogelijk zijn er een maximumbestandsgrootte over het uploaden van binaire via Postman.

De uploadaanvraag voor de is onderdeel van de **Media** verzameling. 

Maken en instellen van een nieuwe aanvraag:
1. Druk op **+** om te maken van een nieuw tabblad in de aanvraag.
2. Selecteer **plaatsen** bewerking en plakken **{{UploadURL}}** in de URL.
2. Laat **autorisatie** tabblad is (niet ingesteld op de **Bearer Token**).
3. In de **Headers** tabblad: **Sleutel**: 'x-ms-blob-type' en **waarde**: "BlockBlob".
2. In de **hoofdtekst** tabblad **binaire**.
4. Kies het bestand met de naam die u hebt opgegeven in de **MediaFileName** omgevingsvariabele.
5. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-upload-file.png)

##  <a name="create-a-metadata-in-the-asset"></a>Maken van een metagegevens in de asset

Zodra het bestand is geüpload, moet u een metagegevens in de asset voor de media-bestand dat u hebt geüpload naar de blob-opslag die is gekoppeld aan uw asset maken.

1. Selecteer **AssetFiles** -> **CreateFileInfos**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-file-info.png)

Het bestand moet worden geüpload en stelt u de metagegevens.

## <a name="validate"></a>Valideren

Om te valideren dat het bestand is geüpload, wilt u mogelijk om op te vragen de [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) en vergelijkt u de **ContentFileSize** (of andere details) aan wat u verwacht te zien in de nieuwe asset. 

Bijvoorbeeld, de volgende **ophalen** bewerking brengt gegevens uit een bestand voor uw assetbestand (of de aanvraag, in het bestand BigBuckBunny.mp4). De query is met behulp van de [omgevingsvariabelen](postman-environment.md) die u eerder hebt ingesteld.

    {{RESTAPIEndpoint}}/Assets('{{LastAssetId}}')/Files

Antwoord bevat grootte, de naam en andere informatie.

    "Id": "nb:cid:UUID:69e72ede-2886-4f2a-8d36-80a59da09913",
    "Name": "BigBuckBunny.mp4",
    "ContentFileSize": "3186542",
    "ParentAssetId": "nb:cid:UUID:0b8f3b04-72fb-4f38-8e7b-d7dd78888938",
            
## <a name="next-steps"></a>Volgende stappen

U kunt nu de geüploade assets coderen. Zie [Assets coderen](media-services-portal-encode.md) voor meer informatie.

U kunt ook Azure Functions gebruiken om een coderingstaak te activeren op basis van een bestand dat binnenkomt in de geconfigureerde container. Zie [dit voorbeeld](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) voor meer informatie.

