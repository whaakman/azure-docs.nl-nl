---
title: Het oplossen van veelvoorkomende problemen tijdens het maken van VHD | Microsoft Docs
description: Antwoorden op veelgestelde vragen voor probleemoplossing en problemen tijdens het maken van de VHD.
services: Azure Marketplace
documentationcenter: ''
author: v-miclar
manager: ''
editor: ''
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ROBOTS: NOINDEX
ms.openlocfilehash: e58a5f8901b2ab53d47095e1d5fd232783e3b1ba
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54079133"
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>Het oplossen van veelvoorkomende problemen aangetroffen tijdens het maken van VHD
In dit artikel is bedoeld om een Azure Marketplace-uitgever en/of CO-beheerder die mogelijk een probleem of Veelgestelde vragen hebben tijdens het publiceren en beheren van hun virtuele machine-oplossingen.

1. Hoe wijzig ik de naam van de host?
   
    Zodra de virtuele machine is gemaakt, kunnen de naam van de host van de gebruikers niet bijwerken.
2. Hoe kan ik de extern bureaublad-service en de aanmeldingswachtwoord opnieuw instellen?
   
   * [Naslaginformatie voor Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Naslaginformatie voor virtuele Linux-machine](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Het genereren van nieuwe ssh-certificaten?
   
   Raadpleeg de koppeling: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Hoe kunt u een open VPN-certificaat configureren?
   
   Raadpleeg de koppeling: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. Wat is het ondersteuningsbeleid voor het uitvoeren van Microsoft-serversoftware in de omgeving met de Microsoft Azure-virtuele machines (infrastructure-as-a-service)?
   
   Raadpleeg de koppeling: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Virtuele Machines hebben een unieke id?
   
   Azure codeert unieke ID van Azure VM in elke virtuele machine. Zie voor meer informatie dit blog en documentatie.
7. In een virtuele machine hoe kan ik beheren de aangepaste scriptextensie in de opstarttaak?
   
   Raadpleeg de koppeling: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Hoe kunt u een virtuele machine maken vanuit Azure portal met behulp van de VHD die is geüpload naar premium storage?
   
   We bieden deze functie nog geen ondersteuning.
9. Is een 32-bits-app in de Azure Marketplace ondersteund?
   
   Raadpleeg de koppeling voor meer informatie over het ondersteuningsbeleid voor: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Telkens wanneer ik wil een installatiekopie maken van mijn VHD's, verschijnt het foutbericht '. VHD is al geregistreerd bij de installatiekopieopslagplaats als de resource' in PowerShell. Een andere afbeelding voordat ik niet hebben gemaakt en heeft ik een afbeelding met deze naam vinden in Azure. Hoe kan ik dit probleem oplossen?
    
    Dit probleem treedt meestal op als de gebruiker een virtuele machine vanaf deze VHD ingericht en er een vergrendeling op deze VHD. Controleer of er geen virtuele machine toegewezen vanaf deze VHD. Als de fout zich blijft voordoen, til een ondersteuningsticket via deze koppeling of van de publicatie portal (details worden vermeld in het antwoord van de vraag 11).