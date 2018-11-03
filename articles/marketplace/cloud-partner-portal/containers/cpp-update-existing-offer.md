---
title: Bijwerken van een bestaande Azure Containers-aanbieding | Microsoft Docs
description: Het bijwerken van een bestaande container-aanbieding op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 2b568717b6656fb9ae15e9a6dbd27441689c4372
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979723"
---
# <a name="update-an-existing-container-offer"></a>Een bestaande container aanbieding bijwerken

In dit artikel wordt stapsgewijs uitgelegd de verschillende aspecten van het bijwerken van uw aanbieding container in de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

Er zijn diverse redenen waarom u wilt mogelijk bijwerken van uw aanbieding, zoals:

-  Een nieuwe versie van de container-installatiekopie toevoegen aan bestaande SKU's.
-  Toevoegen van nieuwe SKU's.
-  Bijwerken van de marketplace-metagegevens voor de aanbieding of de afzonderlijke SKU's.

Om te helpen u bij deze wijzigingen, de portal biedt de **vergelijken** en **geschiedenis** functies.  


## <a name="unpermitted-changes-to-a-container-offer-or-sku"></a>Ongeoorloofde wijzigingen in een container aanbieding of SKU

Er zijn kenmerken van een aanbieding van de container of de SKU die niet kan worden gewijzigd nadat de aanbieding gepubliceerd op Azure Marketplace is. U kunt de volgende instellingen niet wijzigen:

-  **Aanbiedings-ID** en **uitgevers-ID** van de aanbieding
-  **SKU-ID** van bestaande SKU's
-  Versie-tags, bijvoorbeeld: `1.0.1`
-  Facturering/licenties model wijzigingen in bestaande SKU 's

## <a name="common-update-operations"></a>Algemene updatebewerkingen

De volgende updatebewerkingen komen veel voor.

### <a name="update-container-image-version-for-a-sku"></a>De versie van de container-installatiekopie bijwerken voor een SKU

Het is gebruikelijk voor de installatiekopie van een container worden regelmatig bijgewerkt met beveiligingspatches en extra functies. In dit scenario dat u wilt bijwerken van de installatiekopie van de container waarnaar uw SKU verwijst met behulp van de volgende stappen uit:

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. In de **SKU's** tabblad, selecteert u de SKU die zijn gekoppeld aan de container-installatiekopie om bij te werken.
4. Onder **containerinstallatiekopie**, selecteer **+ nieuwe versie van de installatiekopie** om toe te voegen een nieuwe containerinstallatiekopie.
5. Geef de nieuwe container **image versies**. Versie van de installatiekopie moet volgen dezelfde richtlijnen als labels als eerdere versies. Tags van de versie moet de vorm X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Controleer of de nieuwe versie die u opgeeft groter is dan alle vorige versies.
6. Selecteer **publiceren** om de werkstroom voor het publiceren van de nieuwe versie van de container-installatiekopie op de Azure Marketplace te starten.

### <a name="add-a-new-sku"></a>Voeg een nieuwe SKU toe

Gebruik de volgende stappen uit om een nieuwe SKU voor uw aanbieding beschikbaar:

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. Onder de **SKU's** tabblad **toevoegen van nieuwe Voorraadeenheid** en bieden een **SKU-ID** in het pop-upvenster.
4. Publiceren van de container met behulp van de stappen in [publiceren container aanbieding](./cpp-publish-offer.md).
5. Selecteer **publiceren** om de werkstroom voor het publiceren van uw nieuwe SKU te starten.

### <a name="update-offer-marketplace-metadata"></a>Metagegevens van marketplace-aanbieding update

Gebruik de volgende stappen uit om bij te werken van de marketplace-metagegevens die zijn gekoppeld aan uw aanbieding. (Bijvoorbeeld: bedrijfsgegevens naam, logo's en enz.)

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. Ga naar de **Marketplace** tabblad. Volg de instructies in de [publiceren container aanbieding](./cpp-publish-offer.md) artikel zodat wijzigingen in metagegevens bieden.
4. Selecteer **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.

## <a name="compare-feature"></a>Functie vergelijken

Als u wijzigingen in een gepubliceerde aanbieding aanbrengt, kunt u de **vergelijken** functie om te controleren van de wijzigingen die u hebt aangebracht.

### <a name="to-use-the-compare-feature"></a>De functie vergelijken gebruiken:

1. Op elk gewenst moment in het bewerkingsproces vergelijken voor uw aanbieding te selecteren.
2. Side-by-side-versies van marketing activa en metagegevens bekijken.


## <a name="history-of-publishing-actions"></a>Geschiedenis van de publicatie van acties

Historische publicatie-activiteit, selecteer de **geschiedenis** tabblad op de navigatiebalk aan de linkerkant menu balk van Cloud Partner-Portal. Hier ziet u de voorzien van een tijdstempel acties tijdens de levensduur van uw Azure Marketplace-aanbiedingen.