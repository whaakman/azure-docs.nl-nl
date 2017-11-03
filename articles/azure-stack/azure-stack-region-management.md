---
title: Regiobeheer van de in Azure Stack | Microsoft Docs
description: Overzicht van regio management in Azure-Stack.
services: azure-stack
documentationcenter: 
author: efemmano
manager: dsavage
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: efemmano
ms.openlocfilehash: d1310f0cb9a820366ab8712a782785e955a24134
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="region-management-in-azure-stack"></a>Regio management in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack heeft het concept van regio's zijn logische entiteiten bestaat uit de hardwarebronnen die gezamenlijk de Stack van Azure-infrastructuur. U vindt in regiobeheer, alle resources die vereist zijn om te werken is de levensduur van het Azure-Stack-infrastructuur.

Eén geïntegreerde implementatie (aangeduid als een *Azure Stack cloud*) vormt om één regio. Elke Azure-Stack Development Kit heeft één regio, met de naam **lokale**. Als u een tweede Azure Stack geïntegreerd systeem implementeren of van een ander exemplaar van de development kit op afzonderlijke hardware instellen, is deze Azure-Stack cloud een andere regio.

## <a name="information-available-through-the-region-management-tile"></a>Informatie die beschikbaar is via de tegel regio-management
Azure Stack beschikt over een reeks regio management mogelijkheden beschikbaar zijn in de **regio management** tegel. Deze tegel is beschikbaar voor een Azure-Stack-operator in het standaarddashboard in de beheerdersportal. U kunt via deze tegel controleren en bijwerken van uw Azure-Stack regio en de bijbehorende onderdelen, die specifiek zijn.

 ![De regio management tegel](media/azure-stack-manage-region/image1.png)

 Als u een regio in de regio management tegel klikt, kunt u toegang tot de volgende informatie:

  ![Beschrijving van deelvensters op de blade regio-management](media/azure-stack-manage-region/image2.png)

1. **De resource-menu**. Hier kunt u toegang hebt tot specifieke infrastructuur management gebieden en weergeven en beheren van de Gebruikersresources zoals storage-accounts en virtuele netwerken.

2. **Waarschuwingen**. Deze tegel hele systeem waarschuwingen bevat en biedt details over elk van deze waarschuwingen.

3. **Updates**. U kunt de huidige versie van de infrastructuur van uw Azure-Stack weergeven op deze tegel.

4. **Resourceproviders**. Resourceproviders is de plaats voor het beheren van de tenant-functionaliteit van de onderdelen die zijn vereist voor het uitvoeren van de Azure-Stack. Elke resourceprovider wordt geleverd met een administratieve ervaring. Deze ervaring kan waarschuwingen bevatten voor de provider, meetgegevens en andere beheermogelijkheden die specifiek zijn voor de resourceprovider.
 
5. **Infrastructuur functies**. Infrastructuur rollen zijn de onderdelen die nodig zijn voor het uitvoeren van de Azure-Stack. De infrastructuur-rollen die rapporteren van waarschuwingen worden weergegeven. Door te klikken op een rol, ziet u de waarschuwingen die zijn gekoppeld aan de specifieke rol en de rolexemplaren waarop deze rol wordt uitgevoerd. Hoewel er de mogelijkheid om op te starten, starten, of een exemplaar van de rol infrastructuur afgesloten, doen **niet** dit doen in een ontwikkelomgeving kit. Deze opties zijn specifiek bedoeld voor een omgeving met meerdere knooppunten, waarbij er meer dan één rolexemplaar per infrastructuurrol. Een rolexemplaar (met name AzS-Xrp01) opnieuw te starten in de development kit zorgt ervoor dat de instabiliteit van het systeem.

## <a name="next-steps"></a>Volgende stappen
[Status en waarschuwingen in de Azure-Stack controleren](azure-stack-monitor-health.md)

[Beheren van updates in de Azure-Stack](azure-stack-updates.md)






