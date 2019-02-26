---
title: Instellingen voor een Power BI-App bieden - Azure Marketplace bieden | Microsoft Docs
description: Instellingen van de aanbieding voor een Power BI-App-aanbieding voor Microsoft AppSource-marketplace configureren.
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
ms.openlocfilehash: 49bbe5dcf17a9aa20cb47f477c59e7115d29dacc
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56819596"
---
# <a name="power-bi-apps-offer-settings-tab"></a>Power BI-Apps bieden instellingen tabblad

Wanneer u opent de **nieuwe aanbieding** pagina voor service-apps, ziet u eerst de **instellingen bieden** tabblad. U opgeven de primaire id's en de naam voor uw aanbod op dit tabblad. Een sterretje (*) geeft aan dat een vereist veld.

![Tabblad instellingen bieden](./media/offer-settings-tab.png)


## <a name="offer-settings-fields"></a>Velden van aanbod-instellingen 

Op de **instellingen bieden** tabblad, moet u gegevens invoeren in de volgende vereiste velden:

|  Veld        |  Description                                                               |
|---------------|----------------------------------------------------------------------------|
| **Aanbiedings-ID**  | Een unieke id (binnen het uitgeverprofiel van een) voor de aanbieding. Deze id in URL's voor product, Azure Resource Manager-sjablonen, zichtbaar zijn en facturering rapporten. De maximale lengte is 50 tekens. Mag alleen kleine letters, alfanumerieke tekens en streepjes (-). Deze mag niet eindigen met een streepje. Deze id kan niet worden gewijzigd nadat een aanbieding live meteen. Als u Contoso publiceert een aanbieding met aanbiedings-ID `sample-SvcApp`, de aanbieding is de AppSource-URL toegewezen `https://appsource.microsoft.com/marketplace/apps/contoso.sample-SvcApp`.      |
| **Publisher** | De unieke id van uw organisatie in [AppSource](https://appsource.microsoft.com). Alle uw aanbiedingen moeten worden gekoppeld aan uw uitgever-ID. Deze waarde kan niet worden gewijzigd nadat de aanbieding is opgeslagen.                         |
| **Naam**      | Een weergavenaam voor uw aanbieding. Deze naam wordt weergegeven in AppSource en in de Cloud Partner-Portal. De maximale lengte is 50 tekens. Gebruik een merk-naam die wordt herkend voor uw product. Hier de naam van uw organisatie niet worden opgenomen, tenzij de app op de markt met die naam gebracht wordt. Wanneer u deze aanbieding op andere websites en publicaties, gebruikt u dezelfde naam in alle publicaties.    <br/>Als u een aanbieding tijdens de preview-periode voor Power BI-Apps vrijgeven, voegt u de tekenreeks `(Preview)` aan het einde van de naam van uw toepassing, als volgt: `Sample Scv App (Preview)`. |
|     |     |


## <a name="next-steps"></a>Volgende stappen

Klik op het volgende tabblad moet u opgeven [technische info](./cpp-technical-info-tab.md) voor uw aanbieding.
