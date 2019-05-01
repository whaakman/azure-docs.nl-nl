---
title: Instellingen voor virtuele machines bieden tabblad in de Cloud Partner-Portal voor Azure Marketplace
description: Hierin wordt beschreven in het tabblad instellingen bieden is gebruikt bij het maken van een virtuele machine in Azure Marketplace-aanbieding.
services: Azure, Marketplace, Cloud Partner Portal, virtual machine
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 04/25/2019
ms.author: pabutler
ms.openlocfilehash: d361b6b8b08f9556cd57215ebdf82c1bf69d372d
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938156"
---
# <a name="virtual-machine-offer-settings-tab"></a>Tabblad voor virtuele machine-instellingen bieden

De **nieuwe aanbieding** pagina voor virtuele machines wordt geopend in het eerste tabblad met de naam **instellingen bieden**.  

![Pagina met de nieuwe aanbieding voor virtuele machines](./media/publishvm_004.png)


## <a name="offer-settings-fields"></a>Velden van aanbod-instellingen

In de **instellingen bieden** tabblad, moet u de volgende velden bevatten.  Een toegevoegde sterretje (*) op de veldnaam van het geeft aan dat dit vereist is. 

|  **Veld**       |     **Beschrijving**                                                          |
|  ---------       |     ---------------                                                          |
| **Aanbiedings-ID\***   | Een unieke id (binnen het uitgeverprofiel van een) voor de aanbieding. Deze id in URL's voor product, Azure Resource Manager-sjablonen, zichtbaar zijn en facturering rapporten. Het heeft een maximale lengte van 50 tekens bevatten, kan alleen worden samengesteld uit kleine letters, alfanumerieke tekens en streepjes (-), maar mag niet eindigen met een streepje. Dit veld kan niet worden gewijzigd nadat een aanbieding live meteen. <br> Bijvoorbeeld, als Contoso een aanbieding met publiceert aanbiedings-ID **voorbeeld-vm**, krijgt het URL van de Azure Marketplace `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-vm?tab=Overview` . |
| **Publisher\***  | De unieke id van uw organisatie in de Azure Marketplace. Alle uw aanbiedingen moeten worden gekoppeld aan uw uitgever-ID. Deze waarde kan niet worden gewijzigd wanneer de aanbieding is opgeslagen. |
| **Naam\***       | Weergavenaam voor uw aanbieding. Deze naam wordt weergegeven in de Azure Marketplace en in de Cloud Partner-Portal. De naam mag maximaal 50 tekens bevatten. Richtlijnen hier is een herkenbare naam merk voor uw product op te nemen. Hier de naam van uw organisatie niet worden opgenomen, tenzij dat is hoe deze wordt gebracht. Als u deze aanbieding in andere websites en publicaties worden marketing, zorgt u ervoor dat de naam precies hetzelfde voor alle publicaties is. |
|   |   |
 
Klik op **opslaan** nadat u alle velden hebt opgegeven. 


## <a name="next-steps"></a>Volgende stappen

In het volgende tabblad, voegt u [SKU's](./cpp-skus-tab.md) naar uw aanbieding.
