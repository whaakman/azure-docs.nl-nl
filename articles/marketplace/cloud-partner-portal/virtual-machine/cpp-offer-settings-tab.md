---
title: Instellingen voor virtuele machines bieden tabblad in de Cloud Partner-Portal voor Azure Marketplace | Microsoft Docs
description: Hierin wordt beschreven in het tabblad instellingen bieden is gebruikt bij het maken van een virtuele machine in Azure Marketplace-aanbieding.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 43c830bea57a4c6f3b6c56552dbcacaccc75d54e
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639675"
---
# <a name="virtual-machine-offer-settings-tab"></a>Tabblad voor virtuele machine-instellingen bieden

De **nieuwe aanbieding** pagina voor virtuele machines wordt geopend in het eerste tabblad met de naam **instellingen bieden**.  Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is. 

![Pagina met de nieuwe aanbieding voor virtuele machines](./media/publishvm_004.png)

In de **instellingen bieden** tabblad, moet u de volgende vereiste velden opgeven.

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
| **Aanbiedings-ID**       | Een unieke id (binnen het uitgeverprofiel van een) voor de aanbieding. Deze id in URL's voor product, Azure Resource Manager-sjablonen, zichtbaar zijn en facturering rapporten. Het heeft een maximale lengte van 50 tekens bevatten, kan alleen worden samengesteld uit kleine letters, alfanumerieke tekens en streepjes (-), maar mag niet eindigen met een streepje. Dit veld kan niet worden gewijzigd nadat een aanbieding live meteen. <br> Bijvoorbeeld, als Contoso een aanbieding met publiceert aanbiedings-ID **voorbeeld-vm**, krijgt het URL van de Azure Marketplace `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` . |
| **Publisher**     | De unieke id van uw organisatie in de Azure Marketplace. Alle uw aanbiedingen moeten worden gekoppeld aan uw uitgever-ID. Deze waarde kan niet worden gewijzigd wanneer de aanbieding is opgeslagen. |
| **Naam**          | Weergavenaam voor uw aanbieding. Deze naam wordt weergegeven in de Azure Marketplace en in de Cloud Partner-Portal. De naam mag maximaal 50 tekens bevatten. Richtlijnen hier is een herkenbare naam merk voor uw product op te nemen. Hier de naam van uw organisatie niet worden opgenomen, tenzij dat is hoe deze wordt gebracht. Als u deze aanbieding in andere websites en publicaties worden marketing, zorgt u ervoor dat de naam precies hetzelfde voor alle publicaties is. |
|  |  |
 
Klik op **opslaan** uw voortgang opslaan. In het volgende tabblad, voegt u [SKU's](./cpp-skus-tab.md) naar uw aanbieding.
