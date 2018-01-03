---
title: Linux-VM-implementatie-RM oplossen | Microsoft Docs
description: Resource Manager implementatieproblemen bij het maken van een nieuwe virtuele Linux-machine in Azure oplossen
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: aea5db05843b0175b8ef8b713944e12262e33010
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Resource Manager deployment problemen met het maken van een nieuwe virtuele Linux-machine in Azure oplossen
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Zie voor andere VM-implementatieproblemen en vragen [problemen met implementatie Linux virtuele machine in Azure](troubleshoot-deploy-vm.md).
## <a name="collect-activity-logs"></a>Registreert activiteit verzamelen
U start het oplossen van problemen door de activiteitenlogboeken om u te identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over het proces te volgen.

[Implementatiebewerkingen bekijken](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Activiteit-logboeken voor het beheren van Azure-resources bekijken](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** als het besturingssysteem Linux gegeneraliseerd, en deze wordt geüpload en/of met de algemene instelling vastgelegd, wordt niet eventuele fouten. Op dezelfde manier als het besturingssysteem zich Linux gespecialiseerde, en deze wordt geüpload en/of met de instelling voor gespecialiseerde vastgelegd en vervolgens niet eventuele fouten.

**Fouten uploaden:**

**N<sup>1</sup>:** als het besturingssysteem is gegeneraliseerd Linux, en is geüpload als gespecialiseerde, ontvangt u een inrichting time-outfout omdat de virtuele machine in de inrichting stadium is vastgelopen.

**N<sup>2</sup>:** als het besturingssysteem is Linux gespecialiseerde en als gegeneraliseerd is geüpload, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Oplossing:**

Als u wilt zowel deze fouten oplossen, de oorspronkelijke schijf geüpload, beschikbare on-premises, met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke). Als u wilt uploaden als gegeneraliseerd, vergeet niet om uit te voeren - inrichting eerst ervan ongedaan maakt.

**Vastleggen fouten:**

**N<sup>3</sup>:** als het besturingssysteem gegeneraliseerd Linux is, en deze wordt vastgelegd als gespecialiseerde, ontvangt u een inrichting time-outfout omdat de oorspronkelijke virtuele machine kan niet worden gebruikt als deze is gemarkeerd als gegeneraliseerd.

**N<sup>4</sup>:** als het besturingssysteem is Linux gespecialiseerde en deze is vastgelegd, zoals gegeneraliseerd, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord. Ook de oorspronkelijke virtuele machine kan niet worden gebruikt omdat deze is gemarkeerd als gespecialiseerde.

**Oplossing:**

Beide deze fouten oplossen, de huidige installatiekopie verwijderen vanuit de portal en [opnieuw vanaf het huidige VHD's vastleggen](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Aangepaste / galerie / marketplace-installatiekopie; Toewijzingsfout
Deze fout zich voordoet in situaties wanneer de nieuwe VM-aanvraag is vastgemaakt aan een cluster dat de aangevraagde VM-grootte kan niet worden ondersteund of heeft geen beschikbare vrije ruimte voor de aanvraag.

**1 oorzaak:** het cluster kan het aangevraagde VM-grootte niet ondersteunen.

**Oplossing 1:**

* Probeer de aanvraag met een kleinere VM.
* Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
  * Stop de virtuele machines in de beschikbaarheidsset.
    Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  * Nadat de virtuele machines stoppen, moet u de nieuwe virtuele machine maken in de gewenste grootte.
  * De nieuwe virtuele machine eerst te starten en vervolgens selecteert u elk van de gestopte virtuele machines en klik op **Start**.

**2 oorzaak:** het cluster heeft geen gratis resources.

**Oplossing 2:**

* De aanvraag op een later tijdstip opnieuw proberen.
* Als de nieuwe virtuele machine deel van een andere beschikbaarheidsset uitmaken kan
  * Maak een nieuwe virtuele machine in een andere beschikbaarheidsset (in dezelfde regio).
  * De nieuwe virtuele machine toevoegen aan hetzelfde virtuele netwerk.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt wanneer u een gestopte Linux VM starten of het formaat van een bestaande Linux VM in Azure, Zie [problemen bij de implementatie met opnieuw te starten of het formaat van een bestaande virtuele Linux-Machine in Azure Resource Manager oplossen](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

