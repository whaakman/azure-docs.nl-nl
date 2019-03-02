---
title: Bijwerken van een bestaande Azure-toepassing-aanbieding | Microsoft Docs
description: Het bijwerken van een bestaande Azure-toepassing-aanbieding op Azure Marketplace.
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
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: pbutlerm
ms.openlocfilehash: 1019c06afbdab0a6b0e172133da316aea01a5504
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57216888"
---
# <a name="update-an-existing-azure-application-offer"></a>Een bestaande Azure-toepassing-aanbieding bijwerken

Er zijn verschillende soorten updates die u doen voor uw aanbieding wilt mogelijk nadat deze is gepubliceerd en actief is. Eventuele wijzigingen die u in uw nieuwe versie van uw aanbieding aanbrengt moet worden opgeslagen en opnieuw om deze weer in de Marketplace worden gepubliceerd. In dit artikel wordt stapsgewijs uitgelegd de verschillende aspecten van het bijwerken van uw aanbieding van de beheerde toepassing in de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

Er zijn diverse redenen waarom u wilt mogelijk bijwerken van uw aanbieding, zoals:

- Een nieuwe versie van de installatiekopie toevoegen aan bestaande SKU's.
- Toevoegen van nieuwe SKU's.
- Bijwerken van de marketplace-metagegevens voor de aanbieding of de afzonderlijke SKU's.

Om te helpen u bij deze wijzigingen, de portal biedt de **vergelijken** en **geschiedenis** functies.

## <a name="unpermitted-changes-to-an-azure-application-offer-or-sku"></a>Ongeoorloofde wijzigingen in een aanbieding van Azure-toepassing of SKU

Er zijn kenmerken van een aanbieding van de container of de SKU die niet kan worden gewijzigd nadat de aanbieding gepubliceerd op Azure Marketplace is. U kunt de volgende instellingen niet wijzigen:

- Aanbiedings-ID en uitgevers-ID van de aanbieding
- SKU-ID van de bestaande SKU 's
- Versie-tags, bijvoorbeeld: `1.0.1`
- Facturering/licenties model wijzigingen in bestaande SKU 's

## <a name="common-update-operations"></a>Algemene updatebewerkingen

De volgende updatebewerkingen komen veel voor.

### <a name="update-image-version-for-a-sku"></a>Versie van de update-installatiekopie voor een SKU

Het is gebruikelijk voor een afbeelding worden regelmatig bijgewerkt met beveiligingspatches, extra functies, enzovoort. In dit scenario dat u wilt bijwerken van de afbeelding die uw SKU verwijst naar de volgende stappen uit:

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. In de **SKU's** tabblad, selecteert u de SKU die zijn gekoppeld aan de installatiekopie van het om bij te werken.
4. Selecteer **+ nieuwe versie van de installatiekopie** om toe te voegen een nieuwe installatiekopie.
5. Geef de nieuwe versies van een installatiekopie. Versie van de installatiekopie moet volgen dezelfde richtlijnen als labels als eerdere versies. Tags van de versie moet de vorm X.Y.Z, waarbij X, Y en Z gehele getallen zijn. Controleer of de nieuwe versie die u opgeeft groter is dan alle vorige versies.
6. Selecteer **publiceren** om de werkstroom voor het publiceren van de nieuwe versie van de container-installatiekopie op de Azure Marketplace te starten.

### <a name="add-a-new-sku"></a>Voeg een nieuwe SKU toe

Gebruik de volgende stappen uit om een nieuwe SKU voor uw aanbieding beschikbaar:

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. Onder de **SKU's** tabblad **toevoegen van nieuwe Voorraadeenheid** en bieden een **SKU-ID** in het pop-upvenster.
4. Publiceren van de aanbieding met behulp van de stappen in [Azure publiceren toepassingsbieding](./cpp-publish-offer.md).
5. Selecteer **publiceren** om de werkstroom voor het publiceren van uw nieuwe SKU te starten.

### <a name="update-offer-marketplace-metadata"></a>Metagegevens van marketplace-aanbieding update

Gebruik de volgende stappen uit om bij te werken van de marketplace-metagegevens die zijn gekoppeld aan uw aanbieding. (Bijvoorbeeld: bedrijf naam, logo's, enz.)

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. Ga naar de **Marketplace** tabblad. Volg de instructies in [Azure publiceren toepassingsbieding](./cpp-publish-offer.md) metagegevens wijzigingen aanbrengen.
4. Selecteer **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.
 
>[!Note]
>Cloud Solution Providers (CSP) partner kanaal aanmelden is nu beschikbaar.  Raadpleeg [Cloud Solution Providers](../../cloud-solution-providers.md) voor meer informatie over marketingcontactpersonen van uw aanbieding via de Microsoft CSP partner kanalen.

## <a name="deleting-an-existing-offer"></a>Een bestaande aanbieding verwijderen...

Mogelijk wilt u uw aanbieding uit Marketplace verwijderen. Verwijderen van een aanbieding, heeft dit geen invloed op de huidige aankopen van deze aanbieding. Deze aankopen van klanten blijven werken als voorheen. De aanbieding wordt niet zijn echter beschikbaar voor alle nieuwe aankopen nadat de verwijdering voltooid is.

Aanbieding beëindigen is het proces van beëindiging van de service en/of licentieovereenkomst tussen u en uw bestaande klanten.
Richtlijnen en beleidsregels met betrekking tot de aanbieding verwijderen en beëindigen worden bepaald door de Microsoft Marketplace-overeenkomst voor uitgevers (Zie sectie 7) en de beleidsregels voor deelname aan (Zie de sectie 6.2).

Zie voor meer informatie, [verwijderen en aanbieding/SKU van Azure Marketplace](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-managed-app-offer-delete).

## <a name="compare-feature"></a>Functie vergelijken

Wanneer u wijzigingen in een gepubliceerde aanbieding aanbrengt, kunt u de functie vergelijken om te controleren van de wijzigingen die u hebt aangebracht.

De functie vergelijken gebruiken:

1. Op elk gewenst moment in het bewerkingsproces vergelijken voor uw aanbieding te selecteren.
2. Side-by-side-versies van marketing activa en metagegevens bekijken.

## <a name="history-of-publishing-actions"></a>Geschiedenis van de publicatie van acties

Historische publicatie-activiteit, selecteer de **geschiedenis** tabblad op de navigatiebalk aan de linkerkant menu balk van Cloud Partner-Portal. Hier ziet u de voorzien van een tijdstempel acties tijdens de levensduur van uw Azure Marketplace-aanbiedingen.

## <a name="next-steps"></a>Volgende stappen

[Aanbieding voor Azure-toepassing](./cpp-azure-app-offer.md)