---
title: Inhoudssleutels maken met REST | Microsoft Docs
description: Informatie over het maken van de inhoud van de sleutels die beveiligde toegang tot bedrijfsmiddelen bieden.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 95e9322b-168e-4a9d-8d5d-d7c946103745
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako
ms.openlocfilehash: 4e4d01371733a2292c69174e4cb135e995e5e935
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53189095"
---
# <a name="create-content-keys-with-rest"></a>Inhoudssleutels maken met REST
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services kunt u versleutelde activa leveren. Een **ContentKey** biedt veilige toegang tot uw **Asset**s. 

Wanneer u een nieuwe asset maken (bijvoorbeeld, voordat u [bestanden uploaden](media-services-rest-upload-files.md)), kunt u de volgende versleutelingsopties: **StorageEncrypted**, **CommonEncryptionProtected**, of **EnvelopeEncryptionProtected**. 

Wanneer u elementen aan uw clients leveren, kunt u [configureren voor activa dynamisch worden versleuteld](media-services-rest-configure-asset-delivery-policy.md) met een van de volgende twee coderingen: **DynamicEnvelopeEncryption** of **DynamicCommonEncryption**.

Versleutelde assets moeten worden gekoppeld aan **ContentKey**s. In dit artikel wordt beschreven hoe u een inhoudssleutel maken.

Hier volgen de algemene stappen voor het genereren van inhoudssleutels dat u koppelt aan activa die u wilt worden versleuteld. 

1. Willekeurig genereren van een 16-bytes AES-sleutel (voor gemeenschappelijke en envelope versleuteling) of een 32-byte-AES-sleutel (voor versleuteling van opslag). 
   
    Dit is de inhoudssleutel voor uw asset, wat betekent dat alle bestanden die zijn gekoppeld aan die asset moeten dezelfde inhoud sleutel gebruiken tijdens het ontsleutelen. 
2. Roep de [GetProtectionKeyId](https://docs.microsoft.com/rest/api/media/operations/rest-api-functions#getprotectionkeyid) en [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) methoden voor het ophalen van het juiste X.509-certificaat dat moet worden gebruikt voor het versleutelen van uw inhoudssleutel.
3. Codeer uw inhoudssleutel met de openbare sleutel van het X.509-certificaat. 
   
   Media Services .NET SDK gebruikt RSA OAEP bij het uitvoeren van de versleuteling.  U ziet een voorbeeld in de [EncryptSymmetricKeyData functie](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4. Een controlesomwaarde (op basis van de PlayReady-AES-algoritme voor belangrijke controlesom) berekend met behulp van de sleutel-id en een inhoudssleutel maken. Zie voor meer informatie de sectie 'PlayReady AES-sleutel controlesom algoritme' van het Object van de Header PlayReady-document zich [hier](https://www.microsoft.com/playready/documents/).
   
   De volgende .NET-voorbeeld berekent de controlesom met behulp van het GUID-gedeelte van de sleutel-id en de wissen inhoudssleutel.
   
        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
         {
 
             byte[] array = null;
             using (AesCryptoServiceProvider aesCryptoServiceProvider = new AesCryptoServiceProvider())
             {
                 aesCryptoServiceProvider.Mode = CipherMode.ECB;
                 aesCryptoServiceProvider.Key = contentKey;
                 aesCryptoServiceProvider.Padding = PaddingMode.None;
                 ICryptoTransform cryptoTransform = aesCryptoServiceProvider.CreateEncryptor();
                 array = new byte[16];
                 cryptoTransform.TransformBlock(keyId.ToByteArray(), 0, 16, array, 0);
             }
             byte[] array2 = new byte[8];
             Array.Copy(array, array2, 8);
             return Convert.ToBase64String(array2);
         }
5. Maak de inhoudssleutel met de **EncryptedContentKey** (geconverteerd naar base64-gecodeerde tekenreeks), **ProtectionKeyId**, **ProtectionKeyType**,  **ContentKeyType**, en **controlesom** waarden die u in de vorige stappen hebt ontvangen.
6. Koppel de **ContentKey** entiteit met uw **Asset** entiteit via de $links-bewerking.

In dit artikel wordt niet weergegeven voor het genereren van een AES-sleutel, versleutelen van de sleutel en de controlesom berekenen. 

> [!NOTE]
> 
> Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie, [instellen voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over het verbinding maken met de AMS-API [toegang tot de API van Azure Media Services met Azure AD-verificatie](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="retrieve-the-protectionkeyid"></a>De ProtectionKeyId ophalen
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

## <a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>De ProtectionKey voor de ProtectionKeyId ophalen
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

## <a name="create-the-contentkey"></a>De ContentKey maken
Nadat u hebt opgehaald van het X.509-certificaat en de openbare sleutel hebt gebruikt voor het versleutelen van uw inhoudssleutel, maakt u een **ContentKey** entiteit en stel de eigenschap dienovereenkomstig waarden.

Een van de waarden die u wanneer instellen moet maakt u de inhoud is van het type sleutel. Kies een van de volgende waarden:

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }


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


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

