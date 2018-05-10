---
title: Activa in Azure mediaservices | Microsoft Docs
description: In dit artikel geeft een uitleg van wat activa zijn en hoe ze worden gebruikt door Azure Media Services.
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
ms.openlocfilehash: 928d88d51503350abe7df847ce58ff066b987c8e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="assets"></a>Assets

Een **Asset** digitale bestanden (inclusief video, audio, afbeeldingen, verzamelingen miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een asset, kunnen ze in de Media Services-codering en streaming-werkstromen worden gebruikt.

Een asset is toegewezen aan een blobcontainer in de [Azure Storage-account](storage-account-concept.md) en de bestanden in de asset worden opgeslagen als blok-blobs in de container. U kunt werken met de Asset-bestanden in de containers met behulp van de opslag-SDK-clients.

Azure Media Services ondersteunt de Blob-lagen wanneer het account wordt gebruikt voor algemene doeleinden v2 (GPv2) opslag. Met GPv2, kunt u bestanden naar cool of koude opslag verplaatsen. Koude opslag is geschikt voor het archiveren van tussentijds bestanden wanneer deze niet langer nodig is (bijvoorbeeld nadat ze zijn gecodeerd.)

In Media Services v3, kan de invoer van de taak van activa of van HTTP (s) URL's worden gemaakt. Zie het maken van een asset die kan worden gebruikt als invoer voor de taak [maken van een taak voor invoer van een lokaal bestand](job-input-from-local-file-how-to.md).

Lees ook over [storage-accounts in een Media Services](storage-account-concept.md) en [transformaties en taken](transform-concept.md).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand Stream](stream-files-dotnet-quickstart.md)
