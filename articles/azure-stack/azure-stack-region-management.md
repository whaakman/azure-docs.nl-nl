---
title: Regiobeheer in Azure Stack | Microsoft Docs
description: Overzicht van regio management in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 9a10d4fc90b916b3cb1eda7b9bac99c5d5f9deba
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160780"
---
# <a name="region-management-in-azure-stack"></a>Regiobeheer in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack maakt gebruik van het concept van *regio's*, die zijn logische entiteiten bestaat uit de hardwareresources die gezamenlijk de Azure Stack-infrastructuur. In het gebied beheer vindt u alle resources die nodig zijn voor de Azure Stack-infrastructuur is uitgevoerd.

Een geïntegreerd systeemimplementatie (aangeduid als een *Azure Stack-cloud*) vormt een enkele regio. Elke Azure Stack Development Kit heeft één regio, met de naam **lokale**. Als u een tweede geïntegreerde Azure Stack-systeem implementeren of van een ander exemplaar van de development kit op afzonderlijke hardware instellen, is deze Azure Stack-cloud in een andere regio.

## <a name="information-available-through-the-region-management-tile"></a>Informatie is beschikbaar via de tegel van de regio-beheer

Azure Stack is een set met mogelijkheden voor de regio beschikbaar zijn in de **regiobeheer** tegel. Deze tegel is beschikbaar voor Azure Stack-operators op de standaard-dashboard in de beheerdersportal. U kunt via deze tegel controleren en bijwerken van uw Azure Stack-regio en de bijbehorende onderdelen, die specifiek zijn.

![De tegel van de regio-beheer](media/azure-stack-manage-region/image1.png)

Als u een regio in op de **regiobeheer** tegel, u kunt toegang tot de volgende informatie:

![Beschrijving van de deelvensters op de beheerblade van de regio](media/azure-stack-manage-region/image2.png)

1. **Het resourcemenu**. Toegang tot specifieke infrastructuur gebieden, en weergeven en beheren van resources, zoals opslagaccounts en virtuele netwerken van gebruiker.

2. **Waarschuwingen**. Een lijst met systeembrede waarschuwingen en biedt details over elk van deze waarschuwingen.

3. **Updates**. De huidige versie van uw Azure Stack-infrastructuur, beschikbare updates en de historie van updates weergeven. U kunt ook uw geïntegreerd systeem bijwerken.

4. **Resourceproviders**. Beheren van de functionaliteit van gebruiker door de onderdelen die vereist voor het uitvoeren van Azure Stack. Elke resourceprovider wordt geleverd met een administratieve ervaring. Deze ervaring kan waarschuwingen bevatten voor de specifieke provider, metrische gegevens en andere beheermogelijkheden die specifiek zijn voor de resourceprovider.

5. **Infrastructuur rollen**. De onderdelen die nodig zijn om uit te voeren van Azure Stack. De infrastructuur-rollen die rapporteren van waarschuwingen worden weergegeven. Als u een rol selecteert, ziet u de waarschuwingen die zijn gekoppeld aan de rol en de rolinstanties waarop deze rol wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

- [Controleprogramma wordt de status en waarschuwingen in Azure Stack](azure-stack-monitor-health.md)
- [Updates beheren in Azure Stack](azure-stack-updates.md)
