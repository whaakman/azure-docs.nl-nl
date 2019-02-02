---
title: Instellingen voor een Power BI-App bieden - Azure Marketplace bieden | Microsoft Docs
description: Instellingen van de aanbieding voor een aanbieding van Power BI-App voor de Microsoft AppSource-Marketplace configureren.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: pbutlerm
ms.openlocfilehash: fef2455129d94913c5b51a08c04403834861bb48
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666784"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Power BI-Apps bieden instellingen tabblad

De **nieuwe aanbieding** pagina voor service-apps wordt geopend in het eerste tabblad met de naam **instellingen bieden**.  Levert u de primaire id's en de naam voor uw aanbod op dit tabblad.  Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is.

![Tabblad instellingen bieden](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Velden van aanbod-instellingen 

In de **instellingen bieden** tabblad, moet u de volgende vereiste velden opgeven.

|  Veld        |  Description                                                               |
|---------------|----------------------------------------------------------------------------|
| **Aanbiedings-ID**  | Een unieke id (binnen het uitgeverprofiel van een) voor de aanbieding. Deze id in URL's op product: Resource Manager-sjablonen, zichtbaar zijn en facturering rapporten. Het heeft een maximale lengte van 50 tekens bevatten, kan alleen worden samengesteld uit kleine letters, alfanumerieke tekens en streepjes (-), maar mag niet eindigen met een streepje. Dit veld kan niet worden gewijzigd nadat een aanbieding live meteen. Bijvoorbeeld, als Contoso een aanbieding met publiceert aanbiedings-ID `sample-SvcApp`, krijgt het de AppSource-URL `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Publisher** | De unieke id van uw organisatie in [AppSource](https://appsource.microsoft.com). Alle uw aanbiedingen moeten worden gekoppeld aan uw uitgever-ID. Deze waarde kan niet worden gewijzigd wanneer de aanbieding is opgeslagen.                         |
| **Naam**      | Weergavenaam voor uw aanbieding. Deze naam wordt weergegeven in AppSource, maar in de Cloud Partner-Portal. De naam mag maximaal 50 tekens bevatten. Richtlijnen hier is een herkenbare naam merk voor uw product op te nemen. Hier de naam van uw organisatie niet worden opgenomen, tenzij dat is hoe de app is gebracht. Als u deze aanbieding in andere websites en publicaties worden marketing, zorgt u ervoor dat de naam hetzelfde voor alle publicaties is.    <br/>Als u een aanbieding tijdens de preview-periode van Power BI-Apps, de preview-modus, vrijgeven toevoegen de tekenreeks `(Preview)` op de naam van uw toepassing, bijvoorbeeld `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Volgende stappen

In het volgende tabblad, geeft u [technische Info](./cpp-technical-info-tab.md) voor uw aanbieding.
