---
title: Privé-SKU's en -abonnementen | Microsoft Docs
description: Het gebruik van privé-SKU's voor het beheren van de beschikbaarheid van aanbiedingen.
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
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: dcc8b9fef75ba13b607d7fdda5bd55cc710165b9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57837011"
---
<a name="private-skus-and-plans"></a>Privé-SKU's en abonnementen
============

Privé-SKU's kunt u de beschikbaarheid van SKU's beperken tot bepaalde klanten. Wanneer een SKU is gemarkeerd privé zijn, is het niet beschikbaar in alle openbare catalogus met inbegrip van [Azure Marketplace](https://azuremarketplace.microsoft.com) en de [Azure-portal](https://portal.azure.com). In de Azure-portal, kunnen alleen klanten die toegang hebben tot de SKU deze zien. Bovendien wordt deze ook gevraagd dat ze toegang tot persoonlijke aanbiedingen hebben.

>[!NOTE]
>Privé-SKU's beschikken over nieuwe unieke SKU /-Plan-id's om te voorkomen dat een conflict met uw openbare SKU's.

U kunt persoonlijke SKU's gebruiken voor het afhandelen van de volgende scenario's:

1.  Publiceren van software die u wilt dat alleen beschikbaar voor bepaalde klanten openbaar en niet openbaar beschikbaar.
2.  Publiceer variaties van de openbare-software op een aangepaste prijs voor bepaalde klanten.
3.  Publiceer variaties van de openbare software met een aangepaste beschrijving en de voorwaarden (via de nieuwe aanbieding).

Als u wilt dat alleen de prijswijziging, kunt u de schijven van een andere SKU in de dezelfde aanbieding opnieuw te gebruiken. Met persoonlijke SKU's hebt u geen opnieuw indienen schijven voor SKU's.

<a name="mark-a-sku-private"></a>Een SKU persoonlijke markeren
---------------------

Als u een SKU als privégegevens, de optie gevraagd als de SKU privé is in-/ uitschakelen:

![Een SKU markeren als privé](./media/cloud-partner-portal-publish-virtual-machine/markingskuprivate.png)

U kunt opnieuw gebruiken van de schijven in een andere SKU en de prijzen of de beschrijving wijzigen. Als u wilt de schijven gebruiken, selecteert u **Ja** vragen als reactie op de 'kiest, wordt deze SKU opnieuw gebruiken van van een openbare SKU afbeeldingen'.

Als de SKU is gemarkeerd als privé en de aanbieding andere SKU's met herbruikbare schijven heeft, bent u vereist om aan te geven dat de schijven van een andere SKU wordt gebruikgemaakt van de SKU. Bent u ook nodig om op te geven van de doelgroep voor het privé-SKU.

>[!NOTE]
>Nadat deze gepubliceerd, kan niet een openbare SKU privé worden gemaakt.

<a name="select-an-image"></a>Een afbeelding selecteren
------------------

U kunt nieuwe schijven bieden voor de SKU van de persoonlijke of opnieuw gebruiken van de dezelfde schijven die al is opgegeven in een andere SKU, alleen wijzigen de prijzen of beschrijving. Als u wilt de schijven gebruiken, selecteert u **Ja** als een reactie op de vraag 'Biedt deze installatiekopie SKU opnieuw gebruiken van een openbare SKU'.

![Geven de installatiekopie opnieuw gebruiken](./media/cloud-partner-portal-publish-virtual-machine/selectimage1.png)

Nadat u hebt geverifieerd dat de SKU wordt gebruikgemaakt van afbeeldingen uit een andere SKU, kunt u de SKU die de bron van de afbeeldingen identificeren.

De aanwijzingen in het volgende scherm vastleggen weergeven over het identificeren van de persoonlijke SKU opnieuw gebruikt op de installatiekopieën van de geselecteerde SKU:

![Een afbeelding selecteren](./media/cloud-partner-portal-publish-virtual-machine/selectimage2.png)

Wanneer u de aanbieding publiceert, zou de installatiekopieën van de geselecteerde SKU beschikbaar onder de persoonlijke SKU-ID van de aangepaste tarieven/voorwaarden worden gemaakt. De persoonlijke SKU is alleen zichtbaar voor de doelgroep.

Voor updates van installatiekopieën, zou u alleen zijn vereist voor het bijwerken van de onderliggende SKU-installatiekopie. Achter de schermen, worden de installatiekopie voor de persoonlijke SKU ook automatisch bijgewerkt. Op dezelfde manier als u de installatiekopie op de onderliggende SKU verwijderen, zou de installatiekopie ook verwijderd uit de privé-SKU.

<a name="restricting-the-audience"></a>Beperken van de doelgroep
------------------------

Persoonlijke aanbiedingen kunnen worden gevonden en die alleen door de beoogde gebruikers geïmplementeerd.
Momenteel kunnen zoeken met behulp van de abonnement-id's van gebruikers.

Deze abonnementen kunnen worden ingevoerd via een formulier handmatig invoeren **voor maximaal 10 abonnementen**, of door een CSV-bestand uploaden, zodat kan **voor maximaal 20.000 abonnementen**.

Handmatige invoer voor beperkte doelgroep:

![Doelgroep handmatig te beperken](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience1.png)

CSV uploaden voor beperkte doelgroep:

![Gebruik CSV voor het beperken van de doelgroep](./media/cloud-partner-portal-publish-virtual-machine/restrictaudience2.png)

Voorbeelden CSV-bestand voor inhoud:

            Type,Id,Description
            SubscriptionId,7738d703-3135-4e8d-8b81-1e70379abd9d,Private Customer

Wanneer u overschakelt van handmatige invoer naar CSV uploaden weergave of vanuit CSV-bestand naar de handmatige invoer, is de oude lijst van de abonnement-id's met toegang tot de SKU niet behouden. Een waarschuwing wordt weergegeven en de lijst alleen bij het opslaan van de aanbieding wordt overschreven.

<a name="sync-private-subscriptions"></a>Synchronisatie van persoonlijke abonnementen
-------------------------

Als u abonnementen toevoegt in een gepubliceerde aanbieding met een persoonlijke SKU of van Plan zijn, hoeft u niet opnieuw publiceren van de aanbieding voor het toevoegen van informatie van de doelgroep. Een Azure-abonnement-ID (abonnementen en -SKU's) of een Tenant-ID (alleen voor abonnementen) gewoon gebruiken om toe te voegen van de doelgroep.

<a name="previewing-private-offers"></a>Beschikbaar als Private Preview biedt
-------------------------

Tijdens de Preview-versie/fasering stap, alleen de aanbieding op preview-abonnementen kunnen toegang krijgen tot de SKU. Dit is de test fase op dat moment u wat de aanbieding controleren kunt zijbalken eruit zouden voor de betreffende klanten en is standaard voor alle typen van de publicatie.

Het niveau van Preview abonnementen voor toegang tot gefaseerde aanbiedingen van aanbieding:

![Preview-versie abonnement-id 's](./media/cloud-partner-portal-publish-virtual-machine/previewoffer1.png)

Nadat de aanbieding gepubliceerd is, worden alleen de beperkte doelgroep abonnementen (ingevoerd via handmatige invoer- of CSV) kunnen weergeven en implementeren van de persoonlijke SKU. We raden aan dat u **altijd uw eigen abonnementen bevatten in de beperkte doelgroep** voor de persoonlijke SKU voor validatie.

>[!NOTE]
>Foutopsporing, wordt ondersteuning van Microsoft en de engineering-teams ook toegang hebben tot deze persoonlijke aanbiedingen.
