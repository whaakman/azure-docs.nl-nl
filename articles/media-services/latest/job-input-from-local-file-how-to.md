---
title: De invoer van een Azure Media Services-taak maken op basis van een lokaal bestand | Microsoft Docs
description: Dit onderwerp leest het maken van een taak voor invoer van een lokaal bestand.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 94e7192e13397ad8ec973d92f4c538f430c9cd60
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-a-local-file"></a>De invoer van een taak maken op basis van een lokaal bestand

In Media Services-v3 bij het verzenden van taken voor het verwerken van uw video's, hebt u Media Services uitgelegd waar vind ik het invoervideo. De invoer video kan worden opgeslagen als een activum Media-Service in dat geval het maken van een invoer asset op basis van een bestand (lokaal of in Azure Blob-opslag is opgeslagen). Dit onderwerp leest het maken van een taak voor invoer van een lokaal bestand. Zie voor een compleet voorbeeld [github voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code laat zien hoe een invoer-activum maken en deze gebruiken als invoer voor de taak. De functie CreateInputAsset voert de volgende acties:

* De Asset gemaakt 
* Haalt een beschrijfbare [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) voor de [container in opslag](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) van de asset
* Uploadt het bestand naar de container in opslag met de SAS-URL

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Volgende stappen

[Maken van een taak voor invoer van de URL van een HTTP (s)](job-input-from-http-how-to.md).
