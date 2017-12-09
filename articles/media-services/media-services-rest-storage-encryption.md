---
title: Versleutelen van uw inhoud met storage encryption met AMS REST-API
description: Informatie over hoe u uw inhoud codeert met storage encryption met AMS REST-API's.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a0a79f3d-76a1-4994-9202-59b91a2230e0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 3c752573be7c07f800b0dce3d12d4dabd7328922
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="encrypting-your-content-with-storage-encryption"></a>Versleutelen van uw inhoud met versleuteling van opslag

Het is raadzaam voor het versleutelen van uw inhoud lokaal met AES-256-bitsversleuteling en vervolgens uploaden naar Azure Storage wordt bewaard in rust versleuteld.

Dit artikel geeft een overzicht van de versleuteling van opslag AMS en ziet u hoe de inhoud versleuteld opslag te uploaden:

* Maak een inhoudssleutel.
* Maak een Asset. Stel de AssetCreationOption op StorageEncryption bij het maken van de Asset.
  
     Versleutelde activa moeten worden gekoppeld aan inhoud sleutels.
* De inhoudssleutel koppelen aan de asset.  
* De parameters versleutelingsgerelateerde instellen op de AssetFile-entiteiten.

## <a name="considerations"></a>Overwegingen 

Als u een gecodeerde asset opslag leveren wilt, moet u de asset leveringsbeleid voor configureren. Voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en uw inhoud met behulp van het opgegeven leveringsbeleid streams. Zie voor meer informatie [Asset leveringsbeleid configureren](media-services-rest-configure-asset-delivery-policy.md).

Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md). 

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="storage-encryption-overview"></a>Overzicht van de Storage-versleuteling
De versleuteling van het AMS-opslag is van toepassing **AES CTR** modus versleuteling op het hele bestand.  AES-CTR modus is een blokcodering versleutelen van willekeurige lengtegegevens zonder dat nodig is voor de opvulling. Dit werkt door het versleutelen van een blok van de teller met de AES-algoritme en XOR-ing de uitvoer van AES met de gegevens te versleutelen of ontsleutelen.  Het item dat wordt gebruikt door de waarde van de InitializationVector kopiëren naar bytes 0 tot en met 7 van de waarde van het prestatiemeteritem is samengesteld en bytes 8 tot en met 15 van de waarde worden ingesteld op nul. Van het blok 16 bytes teller bytes 8 tot 15 (dat wil zeggen, de minst significante bytes) gebruikt als een eenvoudige 64-bits geheel getal zonder teken die wordt verhoogd met één voor elke volgende blok gegevens verwerkt en wordt opgeslagen in de bytevolgorde netwerk. Als dit geheel getal de maximumwaarde (0xFFFFFFFFFFFFFFFF bereikt) vervolgens en oplopend in stappen wordt de teller blok nul (in bytes 8 tot en met 15) Hiermee stelt u zonder de 64 bits van de teller (dat wil zeggen bytes 0 tot en met 7).   Om de beveiliging van de versleuteling van de modus AES CTR behouden, de InitializationVector-waarde voor een opgegeven sleutel-id voor elke inhoudssleutel moet uniek zijn voor elk bestand en bestanden moet minder zijn dan 2 ^ 64 blokken in lengte.  Dit is om ervoor te zorgen dat een itemwaarde nooit met een opgegeven sleutel wordt opnieuw gebruikt. Zie voor meer informatie over de modus CTR [deze wikipagina](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (het wiki-artikel wordt de term 'Nonce' in plaats van 'InitializationVector' gebruikt).

Gebruik **Opslagversleuteling** voor het versleutelen van uw versleutelde inhoud lokaal via AES-256-bits codering en uploaden naar Azure Storage wordt bewaard in rust versleuteld. Beveiligd met storage encryption activa zijn automatisch niet-versleuteld geplaatst in een versleuteld bestandssysteem voordat ze worden gecodeerd en eventueel opnieuw worden versleuteld voordat geüpload als een nieuwe uitvoerasset. Het primaire gebruiksvoorbeeld storage Encryption is wanneer u wilt uw invoer van hoge kwaliteit mediabestanden beveiligen met sterke codering in rust op schijf.

Voor het leveren van een gecodeerde asset opslag, moet u beleid voor de levering van de asset configureren zodat Media Services weet hoe u wilt uw inhoud leveren. Voordat uw asset kan worden gestreamd, wordt de streaming-server verwijdert u de versleuteling van opslag en streams van uw inhoud met behulp van het opgegeven leveringsbeleid (bijvoorbeeld, AES, common encryption of er wordt geen versleuteling).

## <a name="create-contentkeys-used-for-encryption"></a>ContentKeys gebruikt voor versleuteling maken
Versleutelde activa moeten worden gekoppeld aan de versleutelingssleutel voor opslag. U moet de inhoudssleutel moet worden gebruikt voor versleuteling voordat u de assetbestanden maken. Deze sectie beschrijft het maken van een inhoudssleutel.

Hieronder vindt u algemene stappen voor het genereren van inhoud sleutels die u koppelen aan de activa die u wilt worden versleuteld. 

1. Storage Encryption willekeurig een 32-byte AES-sleutel genereren. 
   
    Dit is de inhoudssleutel voor uw asset, wat betekent dat alle bestanden die zijn gekoppeld aan die asset moeten dezelfde inhoud sleutel gebruiken tijdens het ontsleutelen. 
2. Roep de [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) en [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methoden voor het ophalen van het juiste X.509-certificaat dat moet worden gebruikt om uw inhoud sleutel te versleutelen.
3. Codeer uw inhoud sleutel met de openbare sleutel van het X.509-certificaat. 
   
   Media Services .NET SDK gebruikmaakt van RSA met OAEP bij het uitvoeren van de codering.  U ziet een voorbeeld van een .NET in de [EncryptSymmetricKeyData functie](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Maak een controlesomwaarde berekend met behulp van de sleutel-id en de inhoudssleutel. Het volgende voorbeeld met .NET berekent de controlesom met de GUID-deel van de sleutel-id en de inhoudssleutel wissen.

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

1. Maak de inhoudssleutel met de **EncryptedContentKey** (geconverteerd naar base64-gecodeerde tekenreeks), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**, en **controlesom** waarden die u in de vorige stappen hebt ontvangen.

    Voor de versleuteling van opslag, moeten de volgende eigenschappen worden opgenomen in de aanvraagtekst.

    De eigenschap body aanvraag    | Beschrijving
    ---|---
    Id | De ContentKey-Id die we onszelf genereren met behulp van de volgende indeling hebben: ' nb:kid:UUID:<NEW GUID>'.
    ContentKeyType | Dit is het inhoudstype key als een geheel getal voor deze inhoud sleutel. De waarde 1 voor versleuteling van opslag doorgegeven.
    EncryptedContentKey | We maken een nieuwe inhoud sleutelwaarde die de waarde van een 256-bits (32 bytes). De sleutel is versleuteld met behulp van het X.509-certificaat voor opslag versleuteling we uit Microsoft Azure Media Services ophalen door het uitvoeren van een HTTP GET-aanvraag voor de GetProtectionKeyId en GetProtectionKey methoden. Zie de volgende .NET-code als een voorbeeld: de **EncryptSymmetricKeyData** methode gedefinieerd [hier](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Dit is de key protection-id voor de opslag versleuteling X.509-certificaat dat is gebruikt voor het versleutelen van onze inhoudssleutel.
    ProtectionKeyType | Dit is het versleutelingstype voor de beveiliging sleutel die is gebruikt voor het versleutelen van de inhoudssleutel. Deze waarde is StorageEncryption(1) in ons voorbeeld.
    Controlesom |De controlesom voor berekende MD5 voor de inhoudssleutel. Deze wordt berekend door de inhoud-Id met de inhoudssleutel te versleutelen. De voorbeeldcode laat zien hoe de controlesom berekenen.


### <a name="retrieve-the-protectionkeyid"></a>De ProtectionKeyId ophalen
Het volgende voorbeeld laat zien hoe de ProtectionKeyId, de vingerafdruk van een certificaat voor het certificaat dat moet u bij het versleutelen van uw inhoudssleutel ophalen. Voer deze stap uit om ervoor te zorgen dat u al het juiste certificaat op uw computer.

Aanvraag:

    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.17
    Host: media.windows.net

Antwoord:

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
Het volgende voorbeeld laat zien hoe voor het ophalen van het X.509-certificaat met behulp van de ProtectionKeyId dat u in de vorige stap hebt ontvangen.

Aanvraag:

    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.17
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net

Antwoord:

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
Nadat u hebt opgehaald van het X.509-certificaat en de openbare sleutel voor het versleutelen van uw inhoud sleutel gebruikt, maakt u een **ContentKey** entiteit en stel de eigenschap dienovereenkomstig waarden.

Een van de waarden die u wanneer instellen moet de inhoud maken sleutel is van het type. In geval van de versleuteling van opslag is de waarde '1'. 

Het volgende voorbeeld ziet u het maken van een **ContentKey** met een **ContentKeyType** ingesteld voor de versleuteling van opslag ('1') en de **ProtectionKeyType** ingesteld op '0' om aan te geven dat de sleutel beveiliging Id de vingerafdruk van het X.509-certificaat.  

Aanvraag

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
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

Antwoord:

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
Het volgende voorbeeld laat zien hoe een asset te maken.

**HTTP-aanvraag**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.17
    Host: media.windows.net

    {"Name":"BigBuckBunny" "Options":1}

**HTTP-antwoord**

Als dit lukt, wordt het volgende geretourneerd:

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
Na het maken van de ContentKey koppelen aan uw Asset met behulp van de bewerking $links, zoals wordt weergegeven in het volgende voorbeeld:

Aanvraag:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.17
    Host: media.windows.net

    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Antwoord:

    HTTP/1.1 204 No Content 

## <a name="create-an-assetfile"></a>Een AssetFile maken
De [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) entiteit vertegenwoordigt een video of audio-bestand dat is opgeslagen in een blob-container. Een assetbestand is altijd gekoppeld aan een asset en een asset kan een of meer assetbestanden bevatten. De taak Media Services-Encoder mislukt als een object van het bestand asset niet gekoppeld aan een digitaal bestand in een blob-container is.

Houd er rekening mee dat de **AssetFile** exemplaar en de werkelijke mediabestand zijn twee verschillende objecten. Het exemplaar AssetFile bevat metagegevens over het mediabestand terwijl het mediabestand de werkelijke media-inhoud bevat.

Nadat u uw digitale media-bestand naar een blobcontainer uploadt, gebruikt u de **samenvoegen** HTTP-aanvraag voor de AssetFile bijgewerkt met gegevens over uw mediabestand (niet weergegeven in dit artikel). 

**HTTP-aanvraag**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
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
