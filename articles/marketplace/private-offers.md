---
title: Persoonlijke aanbiedingen | Azure
description: Persoonlijke aanbiedingen op Azure Marketplace voor uitgevers van app- en service.
services: Azure, Marketplace, Compute
documentationcenter: ''
author: qianw211
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/1/2018
ms.author: qianw211
ms.openlocfilehash: 1efe65feaac6e71437958451e8c1a44027495fce
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620010"
---
# <a name="private-offers"></a>Persoonlijke aanbiedingen

Persoonlijke aanbiedingen op [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/) uitgevers te maken van SKU's die alleen zichtbaar voor de betreffende klanten zijn inschakelen.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Enterprise overeenkomsten met persoonlijke aanbiedingen ontgrendelen

Online marktplaatsen zakelijke klanten steeds gebruiken om te zoeken, uitproberen en kopen van cloudoplossingen. Met persoonlijke aanbiedingen kunnen uitgevers nu via marketplace privé aangepaste oplossingen delen met specifieke klanten met mogelijkheden die ondernemingen nodig hebben:

- *Heeft onderhandeld over prijzen* kunt uitgevers kortingen uitbreiden en prijzen van openbaar beschikbare aanbiedingen niet weergegeven.
- *Persoonlijke voorwaarden en bepalingen* uitgevers om aan te passen de voorwaarden en bepalingen aan een specifieke klant inschakelen.
- *Gespecialiseerde configuraties* kunnen uitgevers hun aanbieding virtuele Machines, Azure-toepassingen en SaaS-Apps op een individuele klant behoeften aanpassen. Deze optie kunt ook uitgevers preview om toegang te bieden op nieuwe productfuncties die, voordat u start grotere schaal voor alle klanten.

Persoonlijke aanbiedingen kunnen uitgevers om te profiteren van de schaal en wereldwijde beschikbaarheid van een openbare marketplace, met de flexibiliteit en controle nodig om te onderhandelen over en bieden een aangepaste deals en configuraties. Deze functies open samen, de mogelijkheid tot een sterke enterprise acceptatie van de marktplaatsen in de cloud.  Ondernemingen kunnen nu kopen en verkopen in manieren die ze verwachten en de vraag.

Persoonlijke aanbiedingen zijn nu beschikbaar voor virtuele Machine, Azure-toepassing (geïmplementeerd als oplossingssjablonen of beheerde toepassingen) en SaaS-Apps biedt. Net als openbare aanbiedingen persoonlijke aanbiedingen kunnen worden gemaakt en beheerd de [Cloud Partner-Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus).  Klanten kunnen worden verleend of ingetrokken van toegang tot persoonlijke aanbiedingen in minuten.

## <a name="creating-private-offers-using-skus-and-plans"></a>Het maken van persoonlijke biedt maakt gebruik van SKU's en abonnementen

Voor *nieuwe of bestaande aanbiedingen met openbare-SKU's of abonnementen*, uitgevers kunnen eenvoudig nieuwe, persoonlijke varianten maken door het maken van nieuwe SKU's of abonnementen en ze te markeren als privé.  [Privé-SKU's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus) en abonnementen zijn onderdelen van een aanbieding en zijn alleen zichtbaar en die kunnen worden gekocht door de betreffende klanten. Privé-SKU's en -abonnementen kunnen gebruiken de basisinstallatiekopieën en/of metagegevens die al zijn gepubliceerd voor een openbare SKU of van plan zijn te bieden. Deze optie kan uitgevers meerdere persoonlijke varianten van een openbare aanbieding maken zonder het publiceren van meerdere versies van de dezelfde basisinstallatiekopie en metagegevens bieden. Voor de virtuele Machine en de Azure-toepassing biedt alleen, dat als een persoonlijke SKU een basisinstallatiekopie met een openbare SKU deelt eventuele wijzigingen in de basisinstallatiekopie van de aanbieding wordt doorgegeven in alle openbare en persoonlijke SKU's met behulp van deze basisinstallatiekopie.

Voor *nieuwe aanbiedingen die alleen persoonlijke SKU's of abonnementen bevatten*, uitgevers hun aanbiedingen maken als een andere aanbieding en vervolgens de SKU's of abonnementen markeren als privé. De aanbiedingen die alleen persoonlijke SKU's of abonnementen hebben zich niet kunnen worden gedetecteerd of toegankelijk is via [Azure Marketplace](https://azuremarketplace.microsoft.com) of de [Azure-portal](https://azure.microsoft.com/features/azure-portal/) door klanten die niet gekoppeld aan de aanbieding zijn.

## <a name="targeting-customers-with-private-offers"></a>Die gericht zijn op klanten met een persoonlijke aanbiedingen
Uitgevers kunnen klanten die gebruikmaken van de abonnements-id's zijn gericht voor zowel nieuwe als bestaande persoonlijke aanbiedingen. Uitgevers met behulp van een aanbieding van een virtuele Machine of Azure-toepassing kunnen beperken van de beschikbaarheid van een privé-SKU naar een afzonderlijke Azure-abonnement-ID of een CSV van maximaal 20.000 Azure-abonnement-id's uploaden. Tijdens het gebruik van een persoonlijke aanbieding van SaaS-App, kunnen uitgevers koppelen een Azure-abonnement-ID of tenant-ID voor het beperken van de beschikbaarheid van een privé-abonnement, met behulp van de handmatige methode of CSV-upload-benadering.

Zodra een aanbieding is gecertificeerd en gepubliceerd, kunnen klanten kunnen worden bijgewerkt of verwijderd uit de SKU of van Plan zijn binnen enkele minuten met behulp van de functie voor synchronisatie privé-abonnementen. Op deze manier kunt snel en eenvoudig de lijst van de klanten waarop de persoonlijke SKU of de planning wordt weergegeven zonder recertifying of de aanbieding publiceren bijwerken-uitgevers.

## <a name="deploying-private-offers"></a>Implementatie van persoonlijke aanbiedingen

Persoonlijke aanbiedingen zijn alleen kan worden gedetecteerd via de [Azure-portal](https://azure.microsoft.com/features/azure-portal/) en niet worden weergegeven via [Azure Marketplace](https://azuremarketplace.microsoft.com). Wanneer u bent aangemeld bij de Azure-portal, kunnen klanten de navigatie-element van de Marketplace voor toegang tot hun persoonlijke aanbiedingen selecteren. Persoonlijke aanbiedingen wordt ook weergegeven in de zoekresultaten en kan worden geïmplementeerd via de opdrachtregel en Azure Resource Manager-sjablonen zoals andere aanbiedingen.

![[Persoonlijke aanbiedingen]](./media/marketplace-publishers-guide/private-offer.png)

Persoonlijke aanbiedingen worden ook weergegeven in zoekresultaten. Kijk uit voor de badge 'Persoonlijke'.

## <a name="next-steps"></a>Volgende stappen

Als u wilt profiteren van deze nieuwe mogelijkheden, u kunt aan de slag verkopen op de [Azure Marketplace](https://azuremarketplace.microsoft.com/sell).
