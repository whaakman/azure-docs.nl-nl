---
title: Bestanden uploaden naar een Azure Media Services-account met behulp van REST | Microsoft Docs
description: Informatie over het ophalen van media-inhoud in Media Services door het maken en uploaden van activa.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: juliako
ms.openlocfilehash: 4ba6fdcec8d71326b02d71dbad429be8c2052171
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>Bestanden uploaden naar een Media Services-account met behulp van REST
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 

In Media Services uploadt u de digitale bestanden naar (of neemt u deze op in) een asset. De [Asset](https://docs.microsoft.com/rest/api/media/operations/asset) entiteit kan bevatten, video, audio, afbeeldingen, verzamelingen miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden (en de metagegevens over deze bestanden.)  Nadat de bestanden in de asset zijn geüpload, wordt uw inhoud veilig opgeslagen in de cloud voor verdere verwerking en streaming. 

In deze zelfstudie leert u hoe voor het uploaden van een bestand en de andere bewerking is gekoppeld:

> [!div class="checklist"]
> * Postman instellen voor alle uploadbewerkingen
> * Verbinding met Media Services maken 
> * Een toegangsbeleid maken met de machtiging schrijven
> * Maak een asset
> * Een SAS-locator te maken en de upload-URL
> * Een bestand uploaden naar blob storage met de upload-URL
> * Maken van een metagegevens in de asset voor het mediabestand dat u hebt geüpload

## <a name="prerequisites"></a>Vereisten

- Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [Een Azure Media Services-account maken met de Azure-portal](media-services-portal-create-account.md).
- Controleer de [toegang tot Azure Media Services-API met overzicht van de AAD-authenticatie](media-services-use-aad-auth-to-access-ams-api.md) artikel.
- Configureer **Postman** zoals beschreven in [Postman configureren voor Media Services REST-API-aanroepen](media-rest-apis-with-postman.md).

## <a name="considerations"></a>Overwegingen

De volgende overwegingen wanneer u Media Services REST-API van toepassing:
 
* Bij het openen van entiteiten met Media Services REST-API, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md). <br/>De verzameling Postman is gebruikt in deze zelfstudie zorgt voor het instellen van de benodigde headers.
* Media Services gebruikt de waarde van de eigenschap IAssetFile.Name tijdens het bouwen van URL's voor de streaming-inhoud (bijvoorbeeld http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Om deze reden is procent codering niet toegestaan. De waarde van de **naam** eigenschap kan niet een van de volgende hebben [procent-encoding-gereserveerde tekens](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] '. Bovendien kunnen alleen er een '.' voor de bestandsnaamextensie.
* De lengte van de naam mag niet groter zijn dan 260 tekens zijn.
* Er is een limiet voor de maximale bestandsgrootte die wordt ondersteund voor verwerking in Media Services. Zie [dit](media-services-quotas-and-limitations.md) voor meer informatie over de beperking voor de bestandsgrootte.

## <a name="set-up-postman"></a>Postman instellen

Zie voor stapsgewijze instructies voor het instellen van Postman voor deze zelfstudie, [Postman configureren](media-rest-apis-with-postman.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

1. Verbindingswaarden toevoegen aan uw omgeving. 

    Sommige variabelen die deel van uitmaken de **MediaServices** [omgeving](postman-environment.md) moet handmatig worden ingesteld voordat u met het uitvoeren van bewerkingen die zijn gedefinieerd beginnen kunt in de [verzameling](postman-collection.md).

    Als u de waarden voor de eerste vijf variabelen, Zie [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-import-env.png)
2. Geef de waarde voor de **MediaFileName** omgevingsvariabele.

    Geef de bestandsnaam van de media die u van plan bent om te uploaden. In dit voorbeeld gaan we de BigBuckBunny.mp4 uploaden. 
3. Bekijk de **AzureMediaServices.postman_environment.json** bestand. U ziet dat bijna alle bewerkingen in de verzameling een 'test' script uitvoeren. De scripts worden sommige waarden worden geretourneerd door het antwoord en het juiste omgevingsvariabelen worden ingesteld.

    Bijvoorbeeld de eerste bewerking een toegangstoken ontvangt en stel deze op de **AccessToken** omgevingsvariabele die wordt gebruikt in alle andere bewerkingen.

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
4. Aan de linkerkant van de **Postman** venster, klik op **1. Verificatie van de AAD-token ophalen** -> **ophalen van Azure AD-Token voor de Service-Principal**.

    Het gedeelte van de URL wordt gevuld met de **AzureADSTSEndpoint** omgevingsvariabele (de waarde die u stelt eerder in deze zelfstudie).
    
5. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postment-get-token.png)

    Hier ziet u het antwoord dat 'access_token' bevat. Het script 'test' neemt deze waarde en stelt de **AccessToken** omgevingsvariabele (zoals hierboven beschreven). Als u de omgevingsvariabelen bekijkt, ziet u deze variabele bevat nu de access token (bearer-token)-waarde die wordt gebruikt in de rest van de bewerkingen. 

    Als het token verloopt de stap 'Ophalen van Azure AD-Token voor Service Principal' opnieuw doorlopen. 

## <a name="create-an-access-policy-with-write-permission"></a>Een toegangsbeleid maken met de machtiging schrijven

### <a name="overview"></a>Overzicht 

>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). U moet dezelfde beleids-id gebruiken als u altijd dezelfde dagen/toegangsmachtigingen gebruikt, bijvoorbeeld beleidsregels voor locators die zijn bedoeld om gedurende een lange periode gehandhaafd te blijven (niet-upload-beleidsregels). Raadpleeg [dit artikel](media-services-dotnet-manage-entities.md#limit-access-policies) voor meer informatie.

Voordat u bestanden uploadt naar blobopslag, de toegang beleid rechten instellen voor het schrijven naar een asset. POST een HTTP-aanvraag naar de entiteitset AccessPolicies daarvoor. Een waarde DurationInMinutes tijdens het maken van definiëren of foutbericht een 500 Interne Server terug in het antwoord. Zie voor meer informatie over AccessPolicies [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy).

### <a name="create-an-access-policy"></a>Een toegangsbeleid maken

1. Selecteer **AccessPolicy** -> **AccessPolicy maken voor het uploaden van**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-access-policy.png)

    Het script 'test' opgehaald van de AccessPolicy-Id en de juiste omgevingsvariabele ingesteld.

## <a name="create-an-asset"></a>Maak een asset

### <a name="overview"></a>Overzicht

Een [asset](https://docs.microsoft.com/rest/api/media/operations/asset) is een container voor meerdere typen of sets van objecten in Media Services, waaronder video, audio, afbeeldingen, verzamelingen miniaturen, tekst-nummers en ondertitelingsbestanden bestanden. In de REST-API moet maken van een Asset POST-aanvraag verzenden met Media Services en eigenschapsinformatie over uw asset plaatsen in de aanvraagtekst.

Een van de eigenschappen die u toevoegen kunt bij het maken van een asset is **opties**. U kunt een van de volgende versleutelingsopties opgeven: **geen** (standaard geen versleuteling wordt gebruikt), **StorageEncrypted** (voor inhoud die vooraf zijn versleuteld met versleuteling van opslag van de client-side is), **CommonEncryptionProtected**, of **EnvelopeEncryptionProtected**. Wanneer er een gecodeerde asset, moet u een leveringsbeleid voor configureren. Zie voor meer informatie [configureren van beleid voor de levering asset](media-services-rest-configure-asset-delivery-policy.md).

Als uw asset is versleuteld, moet u een **ContentKey** en koppel deze aan uw asset zoals beschreven in het volgende artikel: [het maken van een ContentKey](media-services-rest-create-contentkey.md). Nadat u de bestanden in de asset uploadt, moet u de eigenschappen van de versleuteling bijwerken op de **AssetFile** entiteit met de waarden die u hebt verkregen tijdens de **Asset** versleuteling. Dit doen met behulp van de **samenvoegen** HTTP-aanvraag. 

In dit voorbeeld maakt u een niet-gecodeerde asset. 

### <a name="create-an-asset"></a>Maak een asset

1. Selecteer **activa** -> **Asset maken**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-asset.png)

    Het script 'test' opgehaald van de activa-Id en de juiste omgevingsvariabele ingesteld.

## <a name="create-a-sas-locator-and-create-the-upload-url"></a>Maken van een SAS-locator en maak de URL uploaden

### <a name="overview"></a>Overzicht

Zodra u hebt de AccessPolicy en Locator ingesteld, wordt het werkelijke bestand geüpload naar een Azure Blob Storage-container met de Azure Storage REST-API's. U moet de bestanden uploaden als blok-blobs. Pagina-blobs worden niet ondersteund door Azure Media Services.  

Zie voor meer informatie over het werken met Azure storage-blobs [REST-API van Blob](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API).

Maak een SAS-Locator (Zie hieronder) voor het ontvangen van de werkelijke upload-URL. Locators definiëren de begintijd en het type van het verbindingseindpunt voor clients die toegang tot bestanden in een Asset wilt. U kunt meerdere Locator-entiteiten voor een bepaalde combinatie van AccessPolicy en Asset voor de afhandeling van aanvragen van andere clients en maken. Elk van deze Locators de waarde StartTime plus de DurationInMinutes-waarde van het AccessPolicy gebruikt om te bepalen hoe lang die een URL kan worden gebruikt. Zie voor meer informatie [Locator](https://docs.microsoft.com/rest/api/media/operations/locator).

Een SAS-URL heeft de volgende indeling:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

### <a name="considerations"></a>Overwegingen

Hierbij geldt het volgende:

* Er kan niet meer dan vijf unieke Locators die tegelijk zijn gekoppeld aan een bepaalde Asset. Zie voor meer informatie Locator.
* Als u uw bestanden direct te uploaden wilt, stelt u de waarde StartTime tot vijf minuten vóór de huidige tijd. Dit is omdat er mogelijk klok scheeftrekken tussen uw client-computer en de Media Services. De waarde StartTime moet ook zijn in de volgende datum/tijd-indeling: jjjj-MM-ssZ (bijvoorbeeld ' 2014-05-23T17:53:50Z ').    
* Er is mogelijk een tweede 30 tot 40 vertraging nadat een Locator wordt gemaakt tot wanneer deze beschikbaar voor gebruik is.

### <a name="create-a-sas-locator"></a>Een SAS-locator te maken

1. Selecteer **Locator** -> **SAS-Locator maken**.
2. Druk op **Verzenden**.

    Het script 'test' maakt de 'Upload-URL op basis van de opgegeven bestandsnaam voor de media en SAS-locator informatie en stelt de juiste omgevingsvariabele.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-sas-locator.png)

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Een bestand uploaden naar blob storage met de upload-URL

### <a name="overview"></a>Overzicht

Nu dat u de upload-URL hebt, moet u het schrijven van code met de Azure Blob-API's direct naar uw bestand uploadt naar de SAS-container. Raadpleeg voor meer informatie de volgende artikelen:

- [Met behulp van de Azure Storage REST-API](https://docs.microsoft.com/azure/storage/common/storage-rest-api-auth?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
- [Blob plaatsen](https://docs.microsoft.com/rest/api/storageservices/put-blob)
- [BLOB's uploaden naar Blob storage](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage)

### <a name="upload-a-file-with-postman"></a>Uploaden van een bestand met Postman

Als voorbeeld gebruiken we Postman een kleine MP4-bestand te uploaden. Mogelijk zijn er een maximale bestandsgrootte op binair via Postman uploaden.

De uploadaanvraag is niet deel uit van de **AzureMedia** verzameling. 

Maken en een nieuwe aanvraag instellen:
1. Druk op  **+** wilt maken van een nieuw tabblad in de aanvraag.
2. Selecteer **plaatsen** bewerking en plakken **{{UploadURL}}** in de URL.
2. Laat **autorisatie** tabblad is (niet ingesteld op de **Bearer-Token**).
3. In de **Headers** tabblad: **sleutel**: 'x-ms-blob-type' en **waarde**: 'BlockBlob'.
2. In de **hoofdtekst** tabblad **binaire**.
4. Kies het bestand met de naam die u hebt opgegeven in de **MediaFileName** omgevingsvariabele.
5. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-upload-file.png)

##  <a name="create-a-metadata-in-the-asset"></a>Een metagegevens in de asset maken

Nadat het bestand zijn geüpload, moet u een metagegevens in de asset voor het mediabestand dat u hebt geüpload naar de blobopslag die is gekoppeld aan uw asset maken.

1. Selecteer **AssetFiles** -> **CreateFileInfos**.
2. Druk op **Verzenden**.

    ![Bestand uploaden](./media/media-services-rest-upload-files/postman-create-file-info.png)

Het bestand moet worden geüpload en stelt u de metagegevens.

## <a name="validate"></a>Valideren

Valideren dat het bestand is geüpload, dat u wilt zoeken de [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) en vergelijkt u de **ContentFileSize** (of andere details) aan wat u verwacht te zien in de nieuwe asset. 

Bijvoorbeeld de volgende **ophalen** bewerking brengt bestandsgegevens voor uw assetbestand (of de aanvraag, in het bestand BigBuckBunny.mp4). De query gebruikt de [omgevingsvariabelen](postman-environment.md) die u eerder hebt ingesteld.

    {{RESTAPIEndpoint}}/Assets('{{LastAssetId}}')/Files

Antwoord bevat de grootte, naam en andere informatie.

    "Id": "nb:cid:UUID:69e72ede-2886-4f2a-8d36-80a59da09913",
    "Name": "BigBuckBunny.mp4",
    "ContentFileSize": "3186542",
    "ParentAssetId": "nb:cid:UUID:0b8f3b04-72fb-4f38-8e7b-d7dd78888938",
            
## <a name="next-steps"></a>Volgende stappen

U kunt nu de geüploade assets coderen. Zie [Assets coderen](media-services-portal-encode.md) voor meer informatie.

U kunt ook Azure Functions gebruiken om een coderingstaak te activeren op basis van een bestand dat binnenkomt in de geconfigureerde container. Zie [dit voorbeeld](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) voor meer informatie.

