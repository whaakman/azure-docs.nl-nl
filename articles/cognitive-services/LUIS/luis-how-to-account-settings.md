---
title: Account en sleutels beheren-LUIS
titleSuffix: Azure Cognitive Services
description: De twee belangrijkste stukjes informatie voor een LUIS-account zijn het gebruikersaccount en de sleutel schrijven. Uw aanmeldings gegevens worden beheerd op account.microsoft.com. Uw bewerkings sleutel wordt beheerd via de pagina met LUIS Portal instellingen.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 9164c94886488ac7dcadeb7894453daad5d8b322
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560479"
---
# <a name="manage-account-and-authoring-key"></a>Account en het ontwerpen van sleutel beheren

De twee belangrijkste stukjes informatie voor een LUIS-account zijn het gebruikersaccount en de sleutel schrijven. Uw aanmeldingsgegevens wordt beheerd op [account.microsoft.com](https://account.microsoft.com). Uw bewerkings sleutel wordt beheerd via de pagina met [Luis](luis-reference-regions.md) Portal **instellingen** .

## <a name="authoring-key"></a>Sleutel ontwerpen

Met deze enkelvoudige ontwerp sleutel voor een specifieke regio, op de pagina **instellingen** , kunt u alle apps in de [Luis](luis-reference-regions.md) -Portal en de [ontwerp-api's](https://go.microsoft.com/fwlink/?linkid=2092087)maken. Als uw gemak de authoring sleutel is toegestaan om een [beperkt](luis-boundaries.md) nummer van het eindpunt van query's per maand.

[![Pagina instellingen van LUIS](./media/luis-how-to-account-settings/account-settings.png)](./media/luis-how-to-account-settings/account-settings.png#lightbox)

De ontwerphandleiding sleutel wordt gebruikt voor alle apps die u bezit, evenals alle apps die u als samenwerker wordt vermeld.

## <a name="authoring-key-regions"></a>Belangrijkste regio's ontwerpen

De sleutel schrijven is specifiek voor de [ontwerpen regio](luis-reference-regions.md#publishing-regions). De sleutel werkt niet in een andere regio.

## <a name="reset-authoring-key"></a>Het ontwerpen van de sleutel opnieuw instellen

Als uw authoring sleutel is geknoeid, opnieuw instellen van de sleutel. De sleutel wordt opnieuw ingesteld voor al uw apps in de [Luis](luis-reference-regions.md) -Portal. Als u uw apps via de API's voor ontwerpen maken, moet u Wijzig de waarde van `Ocp-Apim-Subscription-Key` aan de nieuwe sleutel.

## <a name="delete-account"></a>Account verwijderen

Zie [gegevensopslag en -verwijdering](luis-concept-data-storage.md#accounts) voor informatie over welke gegevens worden verwijderd wanneer u uw account verwijderen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over uw [ontwerpen sleutel](luis-concept-keys.md#authoring-key).

