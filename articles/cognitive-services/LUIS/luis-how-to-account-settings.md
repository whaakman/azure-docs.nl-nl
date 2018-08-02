---
title: Beheren van de instellingen van uw account in LUIS | Microsoft Docs
description: LUIS-website gebruiken voor het beheren van instellingen van uw account.
titleSuffix: Azure
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/08/2018
ms.author: diberry
ms.openlocfilehash: 73e90e5ae86db2c2c4625762b285f8c86f0e241b
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398042"
---
# <a name="manage-account-and-authoring-key"></a>Account en het ontwerpen van sleutel beheren
De twee belangrijkste stukjes informatie voor een LUIS-account zijn het gebruikersaccount en de sleutel schrijven. Uw aanmeldingsgegevens wordt beheerd op [account.microsoft.com](https://account.microsoft.com). Uw authoring sleutel wordt beheerd via de [LUIS](luis-reference-regions.md) website **instellingen** pagina. 

## <a name="authoring-key"></a>Sleutel ontwerpen

Deze één, specifiek ontwerpen sleutel, op de **instellingen** pagina, kunt u de auteur van al uw apps uit de [LUIS](luis-reference-regions.md) website, evenals de [API's ontwerpen](https://aka.ms/luis-authoring-api). Als uw gemak de authoring sleutel is toegestaan om een [beperkt](luis-boundaries.md) nummer van het eindpunt van query's per maand. 

![Pagina instellingen van LUIS](./media/luis-how-to-account-settings/account-settings.png)

De ontwerphandleiding sleutel wordt gebruikt voor alle apps die u bezit, evenals alle apps die u als samenwerker wordt vermeld.

## <a name="authoring-key-regions"></a>Belangrijkste regio's ontwerpen
De sleutel schrijven is specifiek voor de [ontwerpen regio](luis-reference-regions.md#publishing-regions). De sleutel werkt niet in een andere regio. 

## <a name="reset-authoring-key"></a>Het ontwerpen van de sleutel opnieuw instellen
Als uw authoring sleutel is geknoeid, opnieuw instellen van de sleutel. De sleutel wordt opnieuw ingesteld op al uw apps in de [LUIS](luis-reference-regions.md) website. Als u uw apps via de API's voor ontwerpen maken, moet u Wijzig de waarde van `Ocp-Apim-Subscription-Key` aan de nieuwe sleutel. 

## <a name="delete-account"></a>Account verwijderen
Zie [gegevensopslag en -verwijdering](luis-concept-data-storage.md#accounts) voor informatie over welke gegevens worden verwijderd wanneer u uw account verwijderen. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over uw [ontwerpen sleutel](luis-concept-keys.md#authoring-key). 

