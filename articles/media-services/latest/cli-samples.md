---
title: Voorbeelden van Azure CLI - Azure mediaservices | Microsoft Docs
description: Voorbeelden van Azure CLI voor Azure Media Services-service
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 04/15/2018
ms.author: juliako
ms.openlocfilehash: bbf69bdcc92316642f6b37d267cdea2aad920316
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Voorbeelden van Azure CLI voor Azure Media Services

De volgende tabel bevat koppelingen naar Azure CLI-voorbeelden voor Azure Media Services.

|  |  |
|---|---|
|**Account**||
| [Een Media Services-account maken](./scripts/cli-create-account.md) | Een Azure Media Services-account maakt. Maakt ook een service principal die kunnen worden gebruikt voor toegang tot API's om het account programmatisch te beheren. |
| [Opnieuw instellen van accountreferenties](./scripts/cli-reset-account-credentials.md)|Hiermee stelt u referenties voor uw account en de instellingen app.config weer opgehaald.|
|**Activa**||
| [Activa maken](./scripts/cli-create-asset.md)|Hiermee maakt u een Media Services-activum om inhoud te uploaden.|
| [Een bestand uploaden](./scripts/cli-upload-file-asset.md)|Een lokaal bestand naar een opslagcontainer geüpload.|
| [Publiceren van een asset](./scripts/cli-publish-asset.md)| Een Streaming-Locator maakt en Streaming-URL's weer opgehaald. |
| **Transformeert** en **taken**||
| [Transformaties maken](./scripts/cli-create-transform.md)|Toont het maken van transformaties. Transformaties beschrijven een eenvoudige werkstroom van taken voor het verwerken van uw video of audio-bestanden (vaak een "recept' genoemd).<br/> U moet altijd als een transformatie met de gewenste naam controleren en "recept" bestaat al. Zo ja, het opnieuw gebruiken. |
| [Taken maken](./scripts/cli-create-jobs.md)|Verzendt een taak aan een eenvoudige codering transformatie met behulp van HTTPs-URL.|
| [EventGrid maken](./scripts/cli-create-event-grid.md)|Maakt een account niveau raster gebeurtenis abonnement voor de taak de status verandert.|

