---
title: Azure Media Services v3-accounts beheren | Microsoft Docs
description: Als u wilt beheren, coderen, codering, analyseren en streaming van media-inhoud in Azure starten, moet u een Media Services-account maken. In dit artikel wordt uitgelegd hoe u Azure Media Services v3-accounts beheren.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: fa9720c2c29af184016d2903e60520e701b4cf79
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670691"
---
# <a name="manage-azure-media-services-v3-accounts"></a>Azure Media Services v3-accounts beheren

Als u wilt beheren, coderen, codering, analyseren en streaming van media-inhoud in Azure starten, moet u een Media Services-account maken. Als u een Media Services-account gaat maken, moet u de naam van een Azure Storage-accountresource opgeven. De opgegeven opslagaccount wordt gekoppeld aan uw Media Services-account. Het Media Services-account en alle gekoppelde opslagaccounts moeten zich in hetzelfde Azure-abonnement bevinden. Zie voor meer informatie, [opslagaccounts](storage-account-concept.md).

## <a name="moving-a-media-services-account-between-subscriptions"></a>Een Media Services-account verplaatsen tussen abonnementen 

Als u verplaatsen van een Media Services-account naar een nieuw abonnement wilt, moet u eerst de gehele resourcegroep met de Media Services-account aan het nieuwe abonnement verplaatsen. U moet alle gekoppelde resources verplaatsen: Azure Storage-accounts, Azure CDN-profielen, enzovoort. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](../../azure-resource-manager/resource-group-move-resources.md). Net als bij alle resources in Azure, kunnen de resource groep verplaatst enige tijd duren.

> [!NOTE]
> Media Services v3 biedt ondersteuning voor multitenancy-model.

### <a name="considerations"></a>Overwegingen

* Back-ups van alle gegevens in uw account maken voordat u migreert naar een ander abonnement.
* U moet alle Streaming-eindpunten te stoppen en live streamen van resources. Uw gebruikers worden pas weer toegang tot uw inhoud voor de duur van de resource-groep verplaatsen. 

> [!IMPORTANT]
> Start het Streaming-eindpunt niet totdat de verplaatsing voltooid is.

### <a name="troubleshoot"></a>Problemen oplossen 

Als u een Media Services-account of een gekoppeld Azure Storage-account 'verbinding verbreken' na het verplaatsen van de bronnengroep, probeert u de sleutels van de Storage-Account. Als u de sleutels van de Storage-Account de 'verbinding verbroken' status van het Media Services-account niet is opgelost, een nieuwe ondersteuningsaanvraag uit het bestand de "Ondersteuning en probleemoplossing" in het menu in het Media Services-account.  

## <a name="next-steps"></a>Volgende stappen

[Een account maken](create-account-cli-quickstart.md)
