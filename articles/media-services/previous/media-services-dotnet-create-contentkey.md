---
title: Inhoudssleutels maken met .NET
description: Informatie over het maken van de inhoud van de sleutels die beveiligde toegang tot bedrijfsmiddelen bieden.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako
ms.openlocfilehash: af8c2b547e1ce1b15410ee20eb59934559ac41ac
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992157"
---
# <a name="create-contentkeys-with-net-legacy"></a>Inhoudssleutels maken met .NET (verouderd)
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services kunt u maken en het leveren van versleutelde activa. Een **ContentKey** biedt veilige toegang tot uw **Asset**s. 

Wanneer u een nieuwe asset maken (bijvoorbeeld, voordat u [bestanden uploaden](media-services-dotnet-upload-files.md)), kunt u de volgende versleutelingsopties: **StorageEncrypted**, **CommonEncryptionProtected**, of **EnvelopeEncryptionProtected**. 

Wanneer u elementen aan uw clients leveren, kunt u [configureren voor activa dynamisch worden versleuteld](media-services-dotnet-configure-asset-delivery-policy.md) met een van de volgende twee coderingen: **DynamicEnvelopeEncryption** of **DynamicCommonEncryption**.

Versleutelde assets moeten worden gekoppeld aan **ContentKey**s. In dit artikel wordt beschreven hoe u een inhoudssleutel maken.

> [!NOTE]
> Bij het maken van een nieuwe **StorageEncrypted** asset met de Media Services .NET SDK, de **ContentKey** automatisch wordt gemaakt en gekoppeld aan de asset.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
Een van de waarden die u moet ingesteld wanneer een inhoud maken sleutel is het inhoudstype key. Kies een van de volgende waarden. 

```csharp
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
```

## <a id="envelope_contentkey"></a>Type envelop ContentKey maken
Het volgende codefragment maakt een inhoudssleutel van het type envelop versleuteling. Vervolgens wordt de sleutel gekoppeld aan de opgegeven asset.

```csharp
    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

call

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);
```


## <a id="common_contentkey"></a>Gemeenschappelijk type ContentKey maken
Het volgende codefragment maakt een inhoudssleutel van het algemene versleutelingstype. Vervolgens wordt de sleutel gekoppeld aan de opgegeven asset.

```csharp
    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
call

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 
```

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

