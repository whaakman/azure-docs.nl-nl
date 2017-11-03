---
title: Een Azure-Web-App naar een andere resourcegroep verplaatsen | Microsoft Docs
description: Beschrijft de scenario's waarin kunt u Web-Apps en App-Services van een resourcegroep naar een andere.
services: app-service
documentationcenter: 
author: ZainRizvi
manager: erikre
editor: 
ms.assetid: 22f97607-072e-4d1f-a46f-8d500420c33c
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: zarizvi
ms.openlocfilehash: 4d5d6540dd0b51a4f172649a1fd9a2343efeceee
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="move-an-azure-web-app-to-another-resource-group"></a>Een Azure-Web-App naar een andere resourcegroep verplaatsen
U kunt een Web-App en/of de bijbehorende resources verplaatsen naar een andere resourcegroep in hetzelfde abonnement of aan een resourcegroep in een ander abonnement. Dit wordt gedaan als onderdeel van het beheer van resources in Azure. Zie voor meer informatie [verplaatsen Azure-resources aan nieuwe abonnement of resourcegroep](../azure-resource-manager/resource-group-move-resources.md).

## <a name="limitations-when-moving-within-the-same-subscription"></a>Beperkingen bij het verplaatsen van binnen hetzelfde abonnement

Wanneer u een Web-App verplaatst _binnen hetzelfde abonnement_, u kunt de geüploade SSL-certificaten niet verplaatsen. Echter kunt u een Web-App in de nieuwe resourcegroep zonder het verplaatsen van de geüploade SSL-certificaat en de SSL-functionaliteit van uw app werkt nog altijd. 

Als u verplaatsen van het SSL-certificaat met de Web-App wilt, voert u de volgende stappen uit:

1.  Het geüploade certificaat verwijderen uit de Web-App.
2.  Verplaats de Web-App.
3.  Upload het certificaat naar de verplaatste Web-App.

## <a name="limitations-when-moving-across-subscriptions"></a>Beperkingen bij het verplaatsen tussen abonnementen

Wanneer u een Web-App verplaatst _voor abonnementen_, gelden de volgende beperkingen:

- De resourcegroep bestemming mag geen alle bestaande App Service-resources. App Service-resources omvatten:
    - Web Apps
    - App Service-abonnementen
    - Geüploade of geïmporteerde SSL-certificaten
    - App Service-omgevingen
- Alle App Service-resources in de resourcegroep moeten samen worden verplaatst.
- App Service-bronnen kunnen alleen worden verplaatst van de resourcegroep waarin ze werden gemaakt. Als een App Service-resource niet meer in de oorspronkelijke resourcegroep is, deze moet worden teruggezet naar die oorspronkelijke resourcegroep eerst en vervolgens kunnen worden verplaatst tussen abonnementen. 
