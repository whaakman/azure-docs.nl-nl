---
title: Een functie-app maken vanuit de Azure Portal | Microsoft Docs
description: Een nieuwe functie-app in Azure App Service maken vanuit de portal.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 4f9882ebc86b8119e6e683c7442ee3448a8d111b
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Een functie-app maken vanuit de Azure portal

Apps van Azure functie maakt gebruik van de Azure App Service-infrastructuur. Dit onderwerp bevat een functie-app maken in de Azure-portal. Een functie-app is de container die als host fungeert voor de uitvoering van afzonderlijke functies. Wanneer u een functie-app in de App Service plan hosten maakt, kan uw app in de functie alle functies van App Service kunt gebruiken.

## <a name="create-a-function-app"></a>Een functie-app maken

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Wanneer u een functie-app maakt, Geef een geldige **appnaam**, die kunnen alleen letters, cijfers en afbreekstreepjes bevatten. Het onderstrepingsteken (**_**) is niet toegestaan.

Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. De naam van uw opslagaccount moet uniek zijn binnen Azure. 

Nadat de functie-app is gemaakt, kunt u afzonderlijke functies in een of meer verschillende talen. Functies maken [met behulp van de portal](functions-create-first-azure-function.md#create-function), [continue implementatie](functions-continuous-deployment.md), of door [uploaden met FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Service-plannen

Azure Functions heeft twee verschillende serviceplannen: verbruik plannings- en App Service-abonnement. Het plan verbruik wijst automatisch rekencapaciteit wanneer uw code wordt uitgevoerd, kan worden geschaald uit als nodig is om belasting te verwerken, en vervolgens kan worden geschaald in wanneer de code wordt niet uitgevoerd. De App Service-abonnement geeft uw functie app-toegang tot de faciliteiten van App Service. Als de functie-app wordt gemaakt en kan niet op dit moment worden gewijzigd, moet u uw service-abonnement kiezen. Zie voor meer informatie [Kies een Azure-functies die als host fungeert voor plan](functions-scale.md).

Als u van plan bent voor het uitvoeren van JavaScript-functies op een App Service-abonnement, moet u een plan met minder cores kiezen. Zie voor meer informatie de [verwijzing in JavaScript voor functies](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Opslagvereisten voor account

Wanneer u een functie-app in App Service, moet u maken of koppelen aan een algemene Azure Storage-account die ondersteuning biedt voor opslag voor blobs, wachtrijen en tabellen. Intern functies opslagruimte gebruikt voor bewerkingen zoals het beheren van triggers en functies die logboekregistratie. Sommige opslagaccounts bieden geen ondersteuning voor wachtrijen en tabellen, zoals alleen-blob storage-accounts, Azure Premium-opslag en opslagaccounts waarvoor ZRS-replicatie. Deze accounts worden gefilterd uit op de blade Opslagaccount bij het maken van een functie-app.

>[!NOTE]
>Wanneer u het verbruik plan hosten, wordt de functie code en binding configuratiebestanden worden opgeslagen in Azure File storage in de belangrijkste storage-account. Wanneer u de belangrijkste storage-account verwijdert, wordt deze inhoud is verwijderd en kan niet worden hersteld.

Zie voor meer informatie over opslagaccounttypen, [introductie van de Azure Storage-Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services). 

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



