---
title: Het oplossen van veelvoorkomende problemen tijdens het maken van de VHD | Microsoft Docs
description: Antwoorden op veelgestelde vragen voor het oplossen van problemen en problemen tijdens het maken van de VHD.
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: 
editor: 
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>Het oplossen van veelvoorkomende problemen aangetroffen tijdens het maken van de VHD
In dit artikel is bedoeld om een Azure Marketplace-uitgever en/of medebeheerder die mogelijk problemen ervaren of Veelgestelde vragen hebben bij het publiceren en beheren van hun oplossing(en) virtuele machine te geven.

1. Hoe wijzig ik de naam van de host
   
    Zodra VM is gemaakt, kunnen gebruikers de naam van de host kunnen niet bijwerken.
2. Hoe de extern bureaublad-service of de aanmeldings-wachtwoord opnieuw instellen?
   
   * [Naslaginformatie voor Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Naslaginformatie voor virtuele Linux-machine](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Het genereren van nieuwe ssh certificaten?
   
   Raadpleeg de koppeling: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Het configureren van een VPN-certificaat voor openen?
   
   Raadpleeg de koppeling: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Wat is het ondersteuningsbeleid voor Microsoft-serversoftware uitgevoerd in de Microsoft Azure-virtuele machine-omgeving (infrastructure-as-a-service)?
   
   Raadpleeg de koppeling: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Virtuele Machines is beschikt over een unieke id?
   
   Azure codeert Azure VM unieke ID in elke virtuele machine. Zie de informatie in deze documentatie en blog.
7. In een virtuele machine beheren de extensie voor aangepaste scripts in de taak starten?
   
   Raadpleeg de koppeling: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Hoe kan ik een virtuele machine maken vanuit de Azure-portal met behulp van de VHD die is geüpload naar de premium-opslag?
   
   We bieden deze functie nog geen ondersteuning.
9. Is een 32-bits app in Azure Marketplace ondersteund?
   
   Raadpleeg de koppeling voor meer informatie over het ondersteuningsbeleid: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Elke keer dat ik wil een installatiekopie maken van mijn VHD's, verschijnt het foutbericht '. VHD is al geregistreerd bij de installatiekopieopslagplaats als resource' in PowerShell. Geen afbeelding voordat ik niet hebt gemaakt, noch heeft ik een afbeelding met deze naam vinden in Azure. Hoe kan ik dit oplossen?
    
    Dit gebeurt meestal als de gebruiker een virtuele machine uit deze VHD ingericht en er is een vergrendeling op deze VHD. Controleer of er geen virtuele machine vanuit deze VHD toegewezen is. Als de fout nog steeds blijft bestaan, klikt u vervolgens Verhoog een ondersteuningsticket via deze koppeling of van de publicatie portal met betrekking tot dit (details worden gegeven in het antwoord van vraag 11).