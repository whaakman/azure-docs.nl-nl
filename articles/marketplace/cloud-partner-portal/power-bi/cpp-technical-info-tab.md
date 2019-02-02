---
title: Technische informatie voor een aanbieding van Power BI-App - Azure Marketplace | Microsoft Docs
description: Technische informatie velden voor de aanbieding van een Power BI-App configureren voor de Microsoft AppSource-Marketplace.
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
ms.openlocfilehash: 71f226d2ba471a2ccbba3123015697da43d2cf80
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667064"
---
# <a name="power-bi-apps-technical-info-tab"></a>Power BI-Apps technische Info tabblad

De **technische Info** tabblad van de **nieuwe aanbieding** pagina is waarin u het Power BI-installatieprogramma opgeven pakket-URL's en eventuele aanvullende informatie die nodig zijn voor de validatie van de nieuwe aanbieding.  Bij de eerste release zijn alle Power BI-Apps gratis, beschikbaar voor downloaden van AppSource, zonder extra kosten. Als gevolg hiervan niet mogelijk om te definiëren van een voorraad eenheden (SKU's) voor dit aanbiedingtype te houden.

![Tabblad technische gegevens](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Technische informatie velden 

In de **technische Info** tabblad, moet u de volgende velden bevatten.  Een toegevoegde sterretje (*) op het veldlabel geeft aan dat dit vereist is.

|        Veld          |  Description                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **URL van installatieprogramma**     | Het adres is gegenereerd door Power BI wanneer u de app publiceren en het niveau naar productie verhogen.  Zie voor meer informatie over het maken van de URL [service-apps publiceren in Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Validatie-instructies**  |  Optionele tekst instructies (max 3000 tekens) voor de validatie-team van Microsoft om u te helpen bij het configureren, verbinding te maken en testen van uw app, met inbegrip van: standaardconfiguratie-instellingen, testaccounts of parameters die kunnen worden gebruikt voor het testen van de optie 'Verbinding maken met gegevens' enzovoort. Deze informatie is alleen zichtbaar voor de validatieteam en wordt alleen gebruikt voor validatie.  |
| **Deze app gemaakt als een Power BI-inhoudspakket?** | Op dit moment is dit een intern gebruikte veld. De waarde is ingesteld op de standaardwaarde en laat `No`, anders wordt dit veld wijzigen `No` publicatie kan belemmeren.  |  
|  |  |


## <a name="next-steps"></a>Volgende stappen

In de volgende [Storefront Details](./cpp-storefront-details-tab.md) tabblad, biedt u marketing- en juridische informatie voor uw app.

