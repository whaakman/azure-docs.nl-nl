---
title: Veelgestelde vragen over Azure Media Services v3 | Microsoft Docs
description: In dit artikel geeft antwoord op veelgestelde vragen over Azure Media Services-v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 26661a213b5df5424bf9ab9ad799345ae35620ea
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036764"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Veelgestelde vragen over Azure Media Services v3

In dit artikel geeft antwoord op veelgestelde vragen over Azure Media Services (AMS)-v3.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Kan ik de Azure-portal gebruiken om v3-resources te beheren?

Nog niet. U kunt een van de ondersteunde SDK's gebruiken. Zie de zelfstudies en voorbeelden in deze set doc-bestand.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Is er een API voor het configureren van gereserveerde Media-eenheden?

U moet op dit moment AMS v2-API's gebruiken voor het configureren van gereserveerde media-eenheden (zoals beschreven in [mediaverwerking schalen](../previous/media-services-scale-media-processing-overview.md). 

Bij het gebruik van **VideoAnalyzerPreset** en/of **AudioAnalyzerPreset**, Media Services-account ingesteld op 10 S3 gereserveerde Media-eenheden.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>Beschikt over V3 Asset geen concept AssetFile?

De AssetFiles zijn verwijderd uit de AMS-API om te scheiden van Media Services van afhankelijkheid van de Storage-SDK. Opslag, geen Media Services, houdt u nu de informatie die deel uitmaakt van in opslag. 

## <a name="where-did-client-side-storage-encryption-go"></a>Waar is de versleuteling van opslag van de client-side gebleven?

Aangeraden wordt nu serverzijde storage-versleuteling (dit is standaard ingeschakeld). Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>Wat is de aanbevolen methode?

Het is raadzaam om dat het gebruik van HTTP (s) worden opgenomen. Zie voor meer informatie, [HTTP (s) opnemen](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Hoe werkt de paginering?

Media Services biedt ondersteuning voor $top voor resources die ondersteuning bieden voor OData, maar de waarde die is doorgegeven aan $top moet minder dan 1000 (bijvoorbeeld de paginagrootte voor paginering).

Hiermee kunt u ofwel een klein aantal items met behulp van $top (bijvoorbeeld de 100 meest recente items) of om te pagina echter alle items met behulp van paginering. 

Media Services biedt geen ondersteuning voor paginering via de gegevens met een opgegeven gebruiker paginaformaat.

Zie voor meer informatie, [filteren, bestellen en](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Het ophalen van een entiteit in Media Services v3?

V3 is gebaseerd op een uniforme API-gebied, waarmee beheer- en operations-functionaliteit die is gebouwd op **Azure Resource Manager**. In overeenstemming met **Azure Resource Manager**, de namen van voorbeeldresources altijd uniek zijn. U kunt dus alle unieke id-strings (bijvoorbeeld GUID's) gebruiken voor uw resourcenamen. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Overzicht van Media Services v3](media-services-overview.md)
