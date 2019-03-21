---
title: Uw inhoud versleutelen met opslagversleuteling op basis van AMS REST-API
description: Leer hoe u uw inhoud versleutelen met opslagversleuteling op basis van AMS REST-API's.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: ca7f749a04b569d183589fba8c788ce48f29358b
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295552"
---
# <a name="encrypting-your-content-with-storage-encryption"></a>Uw inhoud versleutelen met opslagversleuteling 

Het is raadzaam voor het versleutelen van uw inhoud lokaal via AES-256-bits codering en vervolgens te uploaden naar Azure Storage waar deze zijn opgeslagen in rust versleuteld.

In dit artikel biedt een overzicht van AMS storage-versleuteling en ziet u hoe u de inhoud van de opslag versleuteld uploaden:

* Maak een inhoudssleutel.
* Maak een Asset. Ingesteld op StorageEncryption de AssetCreationOption bij het maken van de Asset.
  
     Versleutelde assets zijn gekoppeld aan inhoud sleutels.
* Koppel de inhoudssleutel aan de asset.  
* De parameters versleuteling-gerelateerde instellen op de AssetFile-entiteiten.

## <a name="considerations"></a>Overwegingen 

Als u een versleutelde activabeheer voor opslag leveren wilt, moet u het leveringsbeleid voor Assets configureren. Voordat uw asset kan worden gestreamd, wordt de server voor streaming Hiermee verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Zie voor meer informatie, [leveringsbeleid voor Assets configureren](media-services-rest-configure-asset-delivery-policy.md).

Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md). 

### <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

|Optie voor opslagversleuteling|Description|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Storage-versleuteling|AES-256-codering, sleutel beheerd door Media Services|Ondersteund<sup>(1)</sup>|Niet ondersteund<sup>(2)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|Niet ondersteund|

<sup>1</sup> terwijl Media Services biedt ondersteuning voor de verwerking van inhoud in de wissen/zonder enige vorm van versleuteling, als dit dus niet aanbevolen.

<sup>2</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="storage-encryption-overview"></a>Overzicht van Storage-versleuteling
De versleuteling van de AMS-opslag is van toepassing **AES-CTR** modus versleuteling op het hele bestand.  AES-CTR modus is een blokcodering versleutelen van willekeurige lengtegegevens zonder dat nodig is voor de opvulling. Deze werkt door het versleutelen van een prestatiemeteritem blok met de AES-algoritme en XOR-ing de uitvoer van AES met de gegevens te versleutelen of ontsleutelen.  Het item dat wordt gebruikt door de waarde van de InitializationVector kopiëren naar bytes 0 tot en met 7 van de waarde van het prestatiemeteritem is samengesteld en bytes 8 tot 15 van de waarde van het prestatiemeteritem zijn ingesteld op nul. Van het blok 16 bytes teller bytes 8 tot 15 (dat wil zeggen, de minst significante bytes) gebruikt als een eenvoudige 64-bits geheel getal zonder teken dat wordt verhoogd door een voor elke volgende gegevensblok verwerkt en wordt opgeslagen in de netwerkbytevolgorde. Als dit geheel getal bereikt de maximale waarde (0xFFFFFFFFFFFFFFFF), stelt deze vervolgens te verhogen de teller blok nul (bytes 8 tot 15) zonder gevolgen voor de 64 bits van de teller (dat wil zeggen bytes 0 tot en met 7).   Als u wilt behouden de beveiliging van de AES-CTR modus versleuteling, de waarde InitializationVector voor een opgegeven sleutel-id voor elke inhoudssleutel moet uniek zijn voor elk bestand en bestanden moeten minder dan 2 ^ 64 blokken in lengte.  Deze unieke waarde is om ervoor te zorgen dat een waarde van het prestatiemeteritem nooit opnieuw wordt gebruikt met een opgegeven sleutel. Zie voor meer informatie over de modus CTR [deze wiki-pagina](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (in de wiki-artikel wordt de term 'Nonce' in plaats van 'InitializationVector' gebruikt).

Gebruik **Opslagversleuteling** voor het versleutelen van uw niet-versleutelde inhoud lokaal via AES-256-bits codering en vervolgens te uploaden naar Azure Storage waar deze zijn opgeslagen in rust versleuteld. Activa die worden beveiligd met storage encryption worden automatisch niet-versleuteld en geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd en eventueel opnieuw worden versleuteld voordat ze worden geüpload weer als een nieuwe uitvoerasset. De primaire use-case voor storage-versleuteling is als u wilt uw invoer media van hoge kwaliteit bestanden beveiligen met sterke versleuteling in rust op schijf.

Voor het leveren van een versleutelde activabeheer voor opslag, moet u het leveringsbeleid voor Assets configureren zodat mediaservices weet hoe u wilt dat uw inhoud kunt leveren. Voordat uw asset kan worden gestreamd, wordt de server voor streaming Hiermee verwijdert u de versleuteling van opslag en gegevensstromen van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld: AES, algemene versleuteling of geen versleuteling).

## <a name="create-contentkeys-used-for-encryption"></a>Gebruikt voor versleuteling Inhoudssleutels maken
Versleutelde assets zijn gekoppeld aan de sleutels voor versleuteling van opslag. De inhoudssleutel moet worden gebruikt voor versleuteling voor het maken van de assetbestanden maken. In deze sectie wordt beschreven hoe u een inhoudssleutel maken.

Hier volgen de algemene stappen voor het genereren van inhoudssleutels dat u koppelt aan activa die u wilt worden versleuteld. 

1. Voor versleuteling van opslag, kunt u een 32-byte-AES-sleutel willekeurig genereren. 
   
    De 32-byte AES-sleutel is de inhoudssleutel voor uw asset, wat betekent dat alle bestanden die zijn gekoppeld aan die asset moeten dezelfde inhoud sleutel gebruiken tijdens het ontsleutelen. 
2. Roep de [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) en [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methoden voor het ophalen van het juiste X.509-certificaat dat moet worden gebruikt voor het versleutelen van uw inhoudssleutel.
3. Codeer uw inhoudssleutel met de openbare sleutel van het X.509-certificaat. 
   
   Media Services .NET SDK gebruikt RSA OAEP bij het uitvoeren van de versleuteling.  Ziet u een .NET-voorbeeld in de [EncryptSymmetricKeyData functie](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Maak een controlesomwaarde berekend met behulp van de sleutel-id en de inhoudssleutel. De volgende .NET-voorbeeld berekent de controlesom met behulp van het GUID-gedeelte van de sleutel-id en de wissen inhoudssleutel.

    ```csharp
            public static string CalculateChecksum(byte[] contentKey, Guid keyId)
            {
                const int ChecksumLength = 8;
                const int KeyIdLength = 16;

                byte[] encryptedKeyId = null;

                // Checksum is computed by AES-ECB encrypting the KID
                // with the content key.
                using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
                {
                    rijndael.Mode = CipherMode.ECB;
                    rijndael.Key = contentKey;
                    rijndael.Padding = PaddingMode.None;

                    ICryptoTransform encryptor = rijndael.CreateEncryptor();
                    encryptedKeyId = new byte[KeyIdLength];
                    encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
                }

                byte[] retVal = new byte[ChecksumLength];
                Array.Copy(encryptedKeyId, retVal, ChecksumLength);

                return Convert.ToBase64String(retVal);
            }
    ```

5. Maak de inhoudssleutel met de **EncryptedContentKey** (geconverteerd naar base64-gecodeerde tekenreeks), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, en **controlesom** waarden die u in de vorige stappen hebt ontvangen.

    Voor de versleuteling van opslag, moeten de volgende eigenschappen in de aanvraagtekst worden opgenomen.

    Hoofdtekst van de aanvraageigenschap    | Description
    ---|---
    Id | De ID ContentKey wordt gegenereerd met behulp van de volgende indeling hebben: "nb:kid:UUID:<NEW GUID>'.
    ContentKeyType | Het inhoudstype key is een geheel getal dat de sleutel wordt gedefinieerd. Voor de indeling van de versleuteling van opslag is de waarde 1.
    EncryptedContentKey | We maken een nieuwe inhoud sleutelwaarde die een waarde is 256-bits (32 bytes). De sleutel is gecodeerd met behulp van de storage-versleuteling X.509-certificaat dat door het uitvoeren van een HTTP GET-aanvraag voor de GetProtectionKeyId en GetProtectionKey methoden van Microsoft Azure Media Services worden opgehaald. Een voorbeeld: Zie de volgende .NET-code: de **EncryptSymmetricKeyData** methode die is gedefinieerd [hier](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Dit is de beveiliging sleutel-ID voor de opslag versleuteling X.509-certificaat dat is gebruikt om onze inhoud sleutel te versleutelen.
    ProtectionKeyType | Dit is het versleutelingstype voor de beveiliging-sleutel die is gebruikt voor het versleutelen van de inhoudssleutel. Deze waarde is StorageEncryption(1) in ons voorbeeld.
    Checksum |De controlesom voor berekende MD5 voor de inhoudssleutel. Deze wordt berekend door het versleutelen van de inhoud-ID met de inhoudssleutel. De voorbeeldcode ziet u hoe u de controlesom berekenen.


### <a name="retrieve-the-protectionkeyid"></a>De ProtectionKeyId ophalen
Het volgende voorbeeld laat zien hoe de ProtectionKeyId, de vingerafdruk van een certificaat voor het certificaat dat moet u bij het versleutelen van uw inhoudssleutel ophalen. Voer deze stap uit om ervoor te zorgen dat u al het juiste certificaat op uw computer hebt.

Aanvraag:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net

Reactie:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

### <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>De ProtectionKey voor de ProtectionKeyId ophalen
Het volgende voorbeeld laat zien hoe de X.509-certificaat met behulp van de ProtectionKeyId dat u in de vorige stap hebt ontvangen op te halen.

Aanvraag:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.17
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Reactie:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>De inhoudssleutel maken
Nadat u hebt opgehaald van het X.509-certificaat en de openbare sleutel hebt gebruikt voor het versleutelen van uw inhoudssleutel, maakt u een **ContentKey** entiteit en stel de eigenschap dienovereenkomstig waarden.

Een van de waarden die u wanneer instellen moet maakt u de inhoud is van het type sleutel. Wanneer u versleuteling van opslag, moet de waarde worden ingesteld op '1'. 

Het volgende voorbeeld ziet u hoe u maakt een **ContentKey** met een **ContentKeyType** instellen voor versleuteling van opslag ("1") en de **ProtectionKeyType** ingesteld op '0' om aan te geven die de key protection-ID is de vingerafdruk van het X.509-certificaat.  

Aanvraag

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }

Reactie:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT

    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Maak een asset
Het volgende voorbeeld ziet hoe u een asset te maken.

**HTTP-aanvraag**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

**HTTP-antwoord**

Als dit lukt, wordt het volgende antwoord geretourneerd:

    HTP/1.1 201 Created
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
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }

## <a name="associate-the-contentkey-with-an-asset"></a>De ContentKey koppelen aan een Asset
Na het maken van de ContentKey, koppel deze aan uw Asset met behulp van de bewerking $links, zoals wordt weergegeven in het volgende voorbeeld:

Aanvraag:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Reactie:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Een AssetFile maken
De [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteit vertegenwoordigt een video of audio-bestand dat is opgeslagen in een blob-container. Een assetbestand is altijd gekoppeld aan een asset en een asset kan een of meer assetbestanden bevatten. De Media Services Encoder-taak mislukt als een object van het bestand asset niet gekoppeld aan een digitaal bestand in een blob-container is.

De **AssetFile** instantie en de werkelijke mediabestand zijn twee verschillende objecten. Het exemplaar AssetFile bevat metagegevens over de mediabestand, terwijl het bestand de werkelijke media-inhoud bevat.

Nadat u uw digitale media-bestand naar een blob-container uploaden, gebruikt u de **samenvoegen** HTTP-aanvraag voor het bijwerken van de AssetFile met informatie over uw media-bestand (niet weergegeven in dit artikel). 

**HTTP-aanvraag**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN>
    x-ms-version: 2.17
    Host: media.windows.net
    Content-Length: 164

    {  
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
