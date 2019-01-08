---
title: Een bestaande aanbieding bijwerken voor Azure Marketplace
description: Een bestaande aanbieding bijwerken voor Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 2bfa10441cf5531c9383527a21b033da26322b34
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073235"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Een bestaande aanbieding bijwerken voor Azure Marketplace 
==============================================

Er zijn verschillende soorten updates die u doen naar uw aanbieding wilt mogelijk als live, bijvoorbeeld:

1.  Toevoegen van nieuwe \"pakket\" naar een bestaande SKU
2.  Nieuwe SKU's toe te voegen aan een bestaande aanbieding
3.  Bijwerken van de metagegevens van de aanbieding/SKU-marketplace

U maakt gebruik van de Cloud Partner-Portal voor het implementeren van deze wijzigingen, en vervolgens uw aanbod opnieuw publiceren. Dit artikel begeleidt u bij de verschillende aspecten van uw Azure-toepassing de aanbieding wordt bijgewerkt.

<a name="unpermitted-changes-to-azure-application-offer"></a>Ongeoorloofde wijzigingen in de aanbieding van Azure-toepassing 
-----------------------------------------------

Er zijn kenmerken van een Azure-toepassing aanbieding/SKU die niet kan worden gewijzigd zodra de aanbieding live in Azure Marketplace meteen.

* Aanbiedings-ID en uitgevers-ID van de aanbieding.
* SKU-ID van de bestaande SKU's.
* Een updatepakket die is gepubliceerd.

<a name="adding-a-new-package-to-an-existing-sku"></a>Een nieuw pakket toe te voegen aan een bestaande SKU 
---------------------------------------

De uitgever kan wilt toevoegen van een nieuwe versie van het pakket om een bestaand pakket bijwerken. Deze toevoeging kan worden gedaan door het uploaden van een nieuw pakket met een andere versienummer.

1.  Aanmelden bij de [Cloud Partner-Portal](http://cloudpartner.azure.com)
2.  In **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken
3.  Op de **SKU's** vormen, klikt u op de SKU die\'s pakket u had willen bijwerken
4.  Klik op \"nieuw pakket\" en geeft u een nieuwe versie
5.  Een nieuw zipbestand met de bijgewerkte sjabloon-bestand uploaden
6.  Klik op publiceren naar een vliegende start de werkstroom publiceren om uw nieuwe Voorraadeenheid live gaan.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Een nieuwe SKU toe te voegen aan een bestaande aanbieding
-------------------------------------

U kunt kiezen om een nieuwe Voorraadeenheid beschikbaar voor uw bestaande aanbieding via de volgende stappen uit:

1.  Meld u aan bij de [Cloud Partner-Portal](http://cloudpartner.azure.com)
2.  In **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken
3.  Op de **SKU's** vormen, klik op nieuwe Voorraadeenheid toevoegen en geef een SKU-ID in het pop-upvenster.
4.  Volg de rest van de stappen zoals opgegeven [hier](./cloud-partner-portal-managed-app-publish.md).
5.  Klik op **publiceren** om te beginnen met de werkstroom publiceren als u wilt dat uw nieuwe Voorraadeenheid live gaan.

<a name="updating-offer-marketplace-metadata"></a>Bijwerken van de metagegevens van Marketplace bieden 
-----------------------------------

Mogelijk hebt u scenario's waarin u bijwerken van de marketplace-metagegevens die zijn gekoppeld aan uw aanbieding wilt zoals het bijwerken van uw bedrijfslogo, enzovoort. Volg de onderstaande stappen.

1.  Meld u aan voor de Cloud Partner-Portal.
2.  In **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken
3.  Ga naar de Marketplace vormen en volg de instructies [hier](../cloud-partner-portal/azure-applications/cpp-marketplace-tab.md) te wijzigen.
4.  Klik op publiceren naar een vliegende start de werkstroom publiceren om uw wijzigingen live gaan.
