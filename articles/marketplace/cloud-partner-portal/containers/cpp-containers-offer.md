---
title: Azure Containers-installatiekopie-aanbieding | Microsoft Docs
description: Overzicht van het proces voor het publiceren van een container-aanbieding op Azure Marketplace.
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
ms.date: 11/02/2018
ms.author: pbutlerm
ms.openlocfilehash: e40e83e16ab2bfd43c3bb5fa38e52a778694e90e
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2018
ms.locfileid: "50979808"
---
# <a name="containers"></a>Containers

<table> <tr> <td>Deze sectie wordt uitgelegd hoe u een containerinstallatiekopie naar publiceert de <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>.  
De container aanbieding type ondersteunt Docker-containerinstallatiekopieën als ingericht <a href="https://docs.microsoft.com/azure/aks/index">Azure Kubernetes Service</a> exemplaren of <a href="https://docs.microsoft.com/azure/container-instances/container-instances-overview">Azure Container Instances</a> en wordt gehost in een <a href="https://docs.microsoft.com/azure/container-registry">Azure Container Registry </a> opslagplaats. </td> <td><img src="./media/container-icon.png"  alt="Azure container icon" /></td> </tr> </table>

## <a name="offer-components"></a>Onderdelen van de aanbieding

In deze sectie geeft een overzicht van de elementen van de publicatie van een container en is bedoeld als richtlijn voor de uitgever in de Azure Marketplace. De publicatie is onderverdeeld in de volgende hoofdonderdelen:

- [Vereisten](./cpp-prerequisites.md) -geeft een lijst van de technische en zakelijke vereisten voor het maken of een container-aanbieding publiceren.
- [Maken van de aanbieding](./cpp-create-offer.md) -vermeldt de stappen die zijn vereist voor het maken van een nieuwe container aanbieding vermelding met behulp van de Cloud Partner-Portal.
- [Voorbereiden van de technische activa](./cpp-create-technical-assets.md) -het maken van de technische assets voor een containeroplossing als een aanbieding op Azure Marketplace.
- [De aanbieding publiceren](./cpp-publish-offer.md) -het indienen van de aanbieding voor publicatie naar de Azure Marketplace.

## <a name="container-publishing-process"></a>Container-publicatieproces

Het volgende diagram illustreert de stappen op hoog niveau in de publicatie van een VM-aanbieding.
![Stappen voor het publiceren van een aanbieding](./media/containers-offer-process.png)

De stappen op hoog niveau voor het publiceren van een container-aanbieding zijn:

1. Maken van de aanbieding - bieden gedetailleerde informatie over de aanbieding. Deze informatie omvat: de beschrijving van de aanbieding, marketingmaterialen, informatie over ondersteuning en assetspecificaties.
2. Maken van de zakelijke en technische activa - maken van de bedrijfsmiddelen (juridische documenten en marketingmaterialen) en de technische activa voor de gekoppelde oplossing (de installatiekopieën van containers die worden gehost in een Azure Container Registry.
3. Maken van de SKU - maken van de SKU('s) die zijn gekoppeld aan de aanbieding. Een unieke SKU is vereist voor elke afbeelding die u van plan bent om te publiceren.
4. Certificeren en publiceren van de aanbieding - nadat de aanbieding en de technische activa zijn voltooid, kunt u de aanbieding indienen. Deze verzending van het publicatieproces te starten. Tijdens dit proces wordt is de oplossing getest, gevalideerd, gecertificeerd, klikt u vervolgens "meteen live' op Azure Marketplace.

## <a name="next-steps"></a>Volgende stappen

Voordat u deze stappen, moet u voldoen aan de [technische en zakelijke vereisten](./cpp-prerequisites.md) voor het publiceren van een container naar de Microsoft Azure Marketplace.