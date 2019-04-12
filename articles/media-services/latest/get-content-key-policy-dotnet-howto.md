---
title: Ophalen van een ondertekeningssleutel van het bestaande beleid met behulp van Media Services v3 .NET SDK - Azure | Microsoft Docs
description: Dit onderwerp leest hoe u een ondertekeningssleutel van het bestaande beleid met behulp van Media Services v3 .NET SDK.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 04/09/2019
ms.author: juliako
ms.openlocfilehash: 49cc2b8c151053377f8f1da0792f10a06695b332
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59496315"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Een ondertekeningssleutel van het bestaande beleid ophalen

Een van de belangrijkste principes van de API v3 is het beter beveiligen van de API. V3 API's retourneren geen geheimen of referenties op **ophalen** of **lijst** bewerkingen. De sleutels zijn altijd null, leeg of opgeschoond uit het antwoord. De gebruiker moet een aparte actie-methode voor het ophalen van geheimen of referenties aan te roepen. De **lezer** rol bewerkingen kan niet worden aangeroepen, zodat bewerkingen zoals Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets kan niet worden aangeroepen. Afzonderlijke acties kunt u meer gedetailleerde RBAC-beveiligingsrechten instellen in een aangepaste rol indien gewenst.

Zie voor meer informatie, [RBAC en Media Services-accounts](rbac-overview.md)

Het voorbeeld in dit artikel ziet hoe u met .NET op een ondertekeningssleutel van het bestaande beleid ophalen. 
 
## <a name="download"></a>Downloaden 

Kloon een GitHub-opslagplaats met het volledige .NET-voorbeeld op de computer met de volgende opdracht:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
De ContentKeyPolicy met geheimen voorbeeld bevindt zich in de [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) map.

## <a name="get-contentkeypolicy-with-secrets"></a>ContentKeyPolicy met geheimen ophalen 

Als u naar de sleutel, gebruikt **GetPolicyPropertiesWithSecretsAsync**, zoals wordt weergegeven in het onderstaande voorbeeld.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Volgende stappen

[Ontwerp van een multi-DRM-beveiliging van inhoud-systeem met toegangsbeheer](design-multi-drm-system-with-access-control.md) 
