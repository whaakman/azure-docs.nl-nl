---
title: Linux VM-implementatie oplossen | Microsoft Docs
description: Bij het maken van een nieuwe virtuele Linux-machine in Azure Resource Manager-implementatieproblemen oplossen
services: virtual-machines-linux, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: jeconnoc
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 9fea914fdf9b025fd5d38219a6bfc81b4a9cc584
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57450277"
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Probleemoplossing voor implementatieproblemen bij het maken van een nieuwe virtuele Linux-machine in Azure Resource Manager
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

Zie voor andere problemen met VM-implementatie en vragen [problemen met implementatie Linux virtuele machine in Azure](troubleshoot-deploy-vm-linux.md).

## <a name="collect-activity-logs"></a>Activiteitenlogboeken verzamelen
Als u wilt gaan met het oplossen van problemen, de activiteitenlogboeken voor het identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over de procedure te volgen.

[Implementatiebewerkingen bekijken](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Activiteitenlogboeken bekijken voor het beheren van Azure-resources](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Als het besturingssysteem gegeneraliseerd Linux is, en deze is geüpload en/of vastgelegd met de algemene instelling, klik zich er niet eventuele fouten. Op dezelfde manier als het besturingssysteem Linux gespecialiseerd, en deze is geüpload en/of vastgelegd met de instelling van gespecialiseerde en eventuele fouten niet.

**Uploadfouten:**

**N<sup>1</sup>:** Als het besturingssysteem Linux gegeneraliseerd en is geüpload als gespecialiseerd, krijgt u een inrichting time-outfout omdat de virtuele machine is vastgelopen bij het inrichtingsproces fase.

**N<sup>2</sup>:** Als het besturingssysteem is Linux, en is geüpload als gegeneraliseerd, wordt er een inrichten is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke de computernaam, gebruikersnaam en wachtwoord.

**Oplossing:**

Uploaden om op te lossen beide deze fouten, de oorspronkelijke VHD, on-premises, met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerde/specifieke) beschikbaar. Als u wilt uploaden als gegeneraliseerd, vergeet niet om uit te voeren - eerst de inrichting.

**Vastleggen fouten:**

**N<sup>3</sup>:** Als het besturingssysteem gegeneraliseerd Linux is, en deze wordt vastgelegd als gespecialiseerd, krijgt u een inrichting time-outfout omdat de oorspronkelijke virtuele machine kan niet worden gebruikt omdat het is gemarkeerd als gegeneraliseerd.

**N<sup>4</sup>:** Als het besturingssysteem is Linux, en deze wordt vastgelegd als gegeneraliseerd, wordt er een inrichten is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke de computernaam, gebruikersnaam en wachtwoord. De oorspronkelijke virtuele machine is ook niet kan worden gebruikt omdat deze is gemarkeerd als gespecialiseerde.

**Oplossing:**

Beide deze fouten oplossen, verwijdert u de huidige installatiekopie vanuit de portal en [opnieuw vanuit de huidige VHD vastleggen](../linux/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerde/specifieke).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Aangepaste / galerie / marketplace-installatiekopie; Toewijzingsfout
Deze fout zich voordoet in situaties als de nieuwe VM-aanvraag is vastgemaakt aan een cluster die op de VM-grootte wordt aangevraagd niet kan ondersteunen, of heeft geen beschikbare vrije ruimte voor de aanvraag.

**1 oorzaak:** Het cluster kan niet de aangevraagde VM-grootte ondersteunt.

**Oplossing 1:**

* Probeer de aanvraag met een kleinere virtuele machine.
* Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
  * Stop alle virtuele machines in de beschikbaarheidsset.
    Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  * Nadat alle virtuele machines stoppen, moet u de nieuwe virtuele machine maken in de gewenste grootte.
  * Start de nieuwe virtuele machine eerst, en vervolgens selecteert u elk van de gestopte VM's en klik op **Start**.

**2 oorzaak:** Het cluster heeft geen gratis resources.

**Oplossing 2:**

* De aanvraag opnieuw uitvoeren op een later tijdstip.
* Als de nieuwe virtuele machine deel van een andere beschikbaarheidsset uitmaken kan
  * Maak een nieuwe virtuele machine in een andere beschikbaarheidsset (in dezelfde regio bevinden).
  * De nieuwe virtuele machine toevoegen aan hetzelfde virtuele netwerk.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt wanneer u een gestopte Linux-VM starten of het formaat van een bestaande VM met Linux in Azure, Zie [problemen met opnieuw te starten of het formaat van een bestaande virtuele Machine voor Linux in Azure Resource Manager oplossen implementatie](../linux/restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

