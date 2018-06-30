---
title: Veelgestelde vragen over Azure Media Services v3 | Microsoft Docs
description: Dit artikel bevat antwoorden op veelgestelde vragen over Azure Media Services-v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/29/2018
ms.author: juliako
ms.openlocfilehash: 098a34aba8e5ce23f64d4bb07e3b9622aa2adb8e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110412"
---
# <a name="azure-media-services-v3-preview-frequently-asked-questions"></a>Azure Media Services v3 Veelgestelde vragen (preview)

Dit artikel bevat antwoorden op veelgestelde vragen over Azure Media Services (AMS)-v3.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Kan ik de Azure portal gebruiken om v3-resources te beheren?

Nog niet. U kunt een van de ondersteunde SDK's gebruiken. Zie zelfstudies en voorbeelden in deze set doc.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Is er een API voor het configureren van gereserveerde Media-eenheden?

Het Media Services-team is RUs in v3 verwijderen. Werk van de service nodig is echter niet voltooid. Tot klanten hebben met de Azure portal of AMS v2 API's RUs ingesteld (zoals beschreven in [media verwerking schalen](../previous/media-services-scale-media-processing-overview.md). 

Wanneer u **VideoAnalyzerPreset** en/of **AudioAnalyzerPreset**, uw Media Services-account ingesteld op 10 S3 Media gereserveerde eenheden.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>Beschikt over V3 Asset geen concept AssetFile?

De AssetFiles zijn verwijderd uit de AMS-API om de Media Services scheiden van afhankelijkheid van de opslag-SDK. Opslag, niet Media Services, nu de informatie die deel uitmaakt van blijft in de opslag. 

## <a name="where-did-client-side-storage-encryption-go"></a>Waar zijn de versleuteling van opslag aan de clientzijde gebleven?

We raden nu aan de serverzijde opslagversleuteling (die standaard is ingeschakeld). Zie voor meer informatie [Azure Storage Service: versleuteling van gegevens in rust](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>Wat is de aanbevolen methode?

Het is raadzaam om dat het gebruik van HTTP (s) is opgenomen. Zie voor meer informatie [HTTP (s) opnemen](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Hoe werkt paginering?

Media Services ondersteunt $top voor bronnen die ondersteuning bieden voor OData, maar de waarde die is doorgegeven aan $top moet minder dan 1000 (bijvoorbeeld de paginagrootte wordt voor paginering).

Hiermee kunt u ofwel een klein aantal artikelen die gebruikmaken van $top (bijvoorbeeld de 100 meest recente items) of om te pagina echter alle items met behulp van paginering. 

Media Services biedt geen ondersteuning voor paginering via de gegevens met een opgegeven gebruiker de systeempaginagrootte.

Zie voor meer informatie [Filtering, rangschikken, paging](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Het ophalen van een entiteit in Media Services v3?

V3 is gebaseerd op een geïntegreerde API-gebied, die zowel beheer van en bewerkingen functionaliteit die zijn gebouwd op **Azure Resource Manager**. In overeenstemming met **Azure Resource Manager**, de resourcenamen altijd uniek zijn. U kunt dus alle unieke id-strings (bijvoorbeeld GUID's) gebruiken voor uw resourcenamen. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Media Services v3-overzicht](media-services-overview.md)
