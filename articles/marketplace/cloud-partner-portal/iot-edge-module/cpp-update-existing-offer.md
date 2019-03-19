---
title: Bijwerken van een bestaande aanbieding voor Azure IoT Edge-module | Microsoft Docs
description: Het bijwerken van een bestaande IoT Edge module aanbieding op Azure Marketplace.
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
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: ca7bed26d91c28304638e85d6da93708bfcfcada
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58170306"
---
# <a name="update-an-existing-iot-edge-module-offer"></a>Een bestaande aanbieding van de IoT Edge-module bijwerken

In dit artikel wordt stapsgewijs uitgelegd de verschillende aspecten van het bijwerken van uw aanbieding IoT Edge-module in de [Cloud Partner-Portal](https://cloudpartner.azure.com/) en vervolgens de aanbieding opnieuw te publiceren.

Er zijn diverse redenen waarom u wilt mogelijk bijwerken van uw aanbieding, zoals:

-  Een nieuwe versie van IoT Edge module installatiekopie toevoegen aan bestaande SKU's.
-  Toevoegen van nieuwe SKU's.
-  Bijwerken van de marketplace-metagegevens voor de aanbieding of de afzonderlijke SKU's.

Om te helpen u bij deze wijzigingen, de portal biedt de **vergelijken** en **geschiedenis** functies.  


## <a name="unpermitted-changes-to-iot-edge-module-offer-or-sku"></a>Ongeoorloofde wijzigingen in IoT Edge module aanbieding of SKU

Er zijn kenmerken van een IoT Edge module aanbieding of de SKU die niet kan worden gewijzigd nadat de aanbieding gepubliceerd op Azure Marketplace is. U kunt de volgende instellingen niet wijzigen:

-  **Aanbiedings-ID** en **uitgevers-ID** van de aanbieding
-  **SKU-ID** van bestaande SKU's
-  Versie-tags, bijvoorbeeld: `1.0.1`
-  Facturering/licenties model wijzigingen in bestaande SKU 's

## <a name="common-update-operations"></a>Algemene updatebewerkingen

De volgende updatebewerkingen komen veel voor.

### <a name="update-the-iot-edge-module-image-version-for-a-sku"></a>Update de versie van de IoT Edge-module-installatiekopie voor een SKU

Het is gebruikelijk voor de installatiekopie van een IoT Edge-module worden regelmatig bijgewerkt met beveiligingspatches en extra functies. In dit scenario dat u wilt bijwerken van de installatiekopie van het IoT Edge-module die de SKU die verwijst naar de volgende stappen uit:

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.

3.  In de **SKU's** tabblad, selecteert u de SKU die zijn gekoppeld aan de installatiekopie van het IoT Edge-module om bij te werken.

4.  Onder **installatiekopie van Edge-module**, selecteer **+ nieuwe versie van de installatiekopie** om toe te voegen een nieuwe installatiekopie van de IoT Edge-module.

5.  Geef de nieuwe IoT Edge-module **image versies**. Versie van de installatiekopie moet volgen dezelfde richtlijnen als labels als eerdere versies. Tags van de versie moet de vorm X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Controleer of de nieuwe versie die u opgeeft groter is dan alle vorige versies.

6.  Selecteer **publiceren** om de werkstroom voor het publiceren van de nieuwe versie van de IoT Edge-module in de Azure Marketplace te starten.

### <a name="add-a-new-sku"></a>Voeg een nieuwe SKU toe

Gebruik de volgende stappen uit om een nieuwe SKU voor uw aanbieding beschikbaar: 

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.

3.  Onder de **SKU's** tabblad **toevoegen van nieuwe Voorraadeenheid** en bieden een **SKU-ID** in het pop-upvenster.

4.  Publiceren van de IoT Edge-module met behulp van de stappen in [publiceren van een IoT Edge-module op Azure Marketplace](./cpp-publish-offer.md).

5.  Selecteer **publiceren** om de werkstroom voor het publiceren van uw nieuwe SKU te starten.


### <a name="update-offer-marketplace-metadata"></a>Metagegevens van marketplace-aanbieding update

Gebruik de volgende stappen uit om bij te werken van de marketplace-metagegevens die zijn gekoppeld aan uw aanbieding. (Bijvoorbeeld: bedrijf naam, logo's, enz.)

1.  Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

2.  Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.

3.  Ga naar de **Marketplace** tabblad. Volg de instructies in de [publiceren van een IoT Edge-module op Azure Marketplace](./cpp-publish-offer.md) artikel om wijzigingen in metagegevens.

4.  Selecteer **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.

## <a name="compare-feature"></a>Functie vergelijken

Als u wijzigingen in een gepubliceerde aanbieding aanbrengt, kunt u de **vergelijken** functie om te controleren van de wijzigingen die u hebt aangebracht. 

**De functie vergelijken gebruiken:**

1.  Op elk gewenst moment tijdens het bewerken, selecteert u **vergelijken** voor uw aanbieding.

    ![Knop vergelijken](./media/iot-edge-module-compare.png)


2.  Side-by-side-versies van marketing activa en metagegevens bekijken.


## <a name="history-of-publishing-actions"></a>Geschiedenis van de publicatie van acties

Historische publicatie-activiteit, selecteer de **geschiedenis** tabblad op de navigatiebalk aan de linkerkant menu balk van Cloud Partner-Portal. Hier ziet u de voorzien van een tijdstempel acties tijdens de levensduur van uw Azure Marketplace-aanbiedingen.  <!-- Need to find correct link here:  legal time windowsFor more information, see [History page](cpp-history-page.md) -->
