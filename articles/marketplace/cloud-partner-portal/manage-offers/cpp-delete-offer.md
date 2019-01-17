---
title: Verwijderen van Marketplace-aanbiedingen - Azure Marketplace | Microsoft Docs
description: Verwijderen van aanbiedingen op Azure en AppSource marktplaatsen, met behulp van de Cloud Partner-Portal
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
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
ms.date: 01/09/2019
ms.author: pbutlerm
ms.openlocfilehash: 7b4ee33d3e231dc59ce1d7b4ae6337efb11e96bc
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355564"
---
# <a name="delete-azure-marketplace-and-appsource-offers-or-skus"></a>Aanbiedingen voor Azure Marketplace en AppSource of SKU's verwijderen

Diverse redenen zijn waarom besluiten u om in te trekken van uw aanbieding uit de Microsoft marketplace, dat kan twee vormen aannemen:

- *Verwijdering bieden* zorgt ervoor dat nieuwe klanten niet meer kunnen kopen of implementeren van uw aanbieding, maar heeft geen invloed op bestaande klanten, die u op basis van uw licentieovereenkomst en de relevante wetten ondersteunen moet. 
- *Beëindiging bieden* is het proces van beëindiging van de service en/of licentieovereenkomst tussen u en uw bestaande klanten. Richtlijnen en beleidsregels met betrekking tot de aanbieding verwijderen en beëindigen vallen onder [Microsoft Marketplace-overeenkomst voor uitgevers](https://go.microsoft.com/fwlink/?LinkID=699560) (sectie 7) en de [beleidsregels voor deelname aan](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (sectie 6.2). 

In dit artikel praat over de verschillende scenario's voor verwijdering en de stappen die nodig zijn om uit te voeren op elk ondersteund.  

> [!NOTE]
> U kunt een aanbieding die niet is gepubliceerd door eenvoudig te selecteren verwijderen de **verwijderen** knop op de werkbalk van de **Editor** tabblad.


## <a name="delete-a-published-sku-from-the-azure-marketplace"></a>Een gepubliceerde SKU verwijderen uit de Azure Marketplace

U kunt een gepubliceerde SKU verwijderen uit Azure Marketplace met behulp van de volgende stappen uit:

1.  Aanmelden bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2.  In de **alle aanbiedingen** pagina, selecteert u uw aanbieding.  Uw aanbieding moet worden weergegeven in de **Editor** tabblad.
3.  Selecteer in de werkbalk links het **SKU's** tabblad. 
4.  Selecteer de SKU die u wilt verwijderen en klik op de **verwijderen** knop.
5.  [Opnieuw publiceren](./cpp-publish-offer.md) de aanbieding op Azure Marketplace.

Nadat de gewijzigde aanbieding wordt gepubliceerd naar de Azure Marketplace, worden de geselecteerde SKU wordt niet meer weergegeven in de Azure Marketplace en de Azure portal.


## <a name="roll-back-to-a-previous-sku-version"></a>Ga terug naar een eerdere versie van de SKU

U kunt de huidige versie van een gepubliceerde SKU van Azure Marketplace verwijderen met behulp van de stappen hier. Wanneer het proces voltooid is, is de SKU teruggedraaid naar de vorige versie.

1. Aanmelden bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. In de **alle aanbiedingen** pagina, selecteert u uw aanbieding.  Uw aanbieding moet worden weergegeven in de **Editor** tabblad.
3. Selecteer in de werkbalk links het **SKU's** tabblad. 
4. De nieuwste versie van de asset gekoppelde oplossing verwijderen uit de lijst met versies van de schijf.  Afhankelijk van het aanbiedingtype, dit veld kan worden **schijf versie**, **Pakketversies**, of vergelijkbare asset. 
5. [Opnieuw publiceren](./cpp-publish-offer.md) de aanbieding op Azure Marketplace.

Nadat de gewijzigde aanbieding op Marketplace theAzure is gepubliceerd, wordt de huidige versie van de vermelde SKU wordt niet meer weergegeven. in de Azure Marketplace en de Azure-portal.  De SKU is teruggedraaid naar de vorige versie.


## <a name="delete-a-live-offer"></a>Een live aanbieding verwijderen

Er zijn verschillende procedures-, bedrijfs- en juridische aspecten voor het verwijderen van een live-aanbieding. Volg de volgende stappen voor begeleiding van het ondersteuningsteam van een live aanbieding verwijderen uit de Azure Marketplace:

1.  Verhoogt een ondersteuning ticket met de [maken van een incident](https://go.microsoft.com/fwlink/?linkid=844975) pagina of door te klikken op **ondersteunen** in de rechterbovenhoek van de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Selecteer het specifieke aanbiedings-type in de **probleemtype** lijst en selecteer **verwijderen van een gepubliceerde aanbieding** in de **categorie** lijst.

3.  De aanvraag indienen.

Het ondersteuningsteam begeleidt u bij de verwijdering van de aanbieding.

> [!NOTE]
> Verwijderen van een aanbieding (of SKU), heeft dit geen invloed op de huidige aankopen van deze aanbieding (of SKU). Deze aankopen blijven werken als voorheen. Echter, verwijderde aanbiedingen of SKU's niet beschikbaar voor alle aankopen in de toekomst.


## <a name="next-steps"></a>Volgende stappen

Nadat u bekend met de basisbewerkingen gebruikt voor het beheren van aanbiedingen bent, bent u klaar om te maken van een exemplaar van een Microsoft [marketplace-aanbieding](../cpp-marketplace-offers.md).
