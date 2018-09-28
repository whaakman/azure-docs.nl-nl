---
title: Windows-VM-implementatie in Azure oplossen | Microsoft Docs
description: Bij het maken van een nieuwe Windows virtuele machine in Azure Resource Manager-implementatieproblemen oplossen
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: JiangChen79
manager: jeconnoc
editor: ''
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 24a12c9144535fecd23be432ee33402eb6528b28
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413537"
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Probleemoplossing voor implementatieproblemen bij het maken van een nieuwe Windows-VM in Azure
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Zie voor andere problemen met VM-implementatie en vragen [problemen met implementatie Windows virtuele machine in Azure](troubleshoot-deploy-vm-windows.md).

## <a name="collect-activity-logs"></a>Activiteitenlogboeken verzamelen
Als u wilt gaan met het oplossen van problemen, de activiteitenlogboeken voor het identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over de procedure te volgen.

[Implementatiebewerkingen bekijken](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Activiteitenlogboeken bekijken voor het beheren van Azure-resources](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** als het besturingssysteem gegeneraliseerd is, Windows en het is geüpload en/of vastgelegd met de algemene instelling, wordt er niet eventuele fouten. Op dezelfde manier als het besturingssysteem Windows gespecialiseerd, en deze is geüpload en/of vastgelegd met de instelling van gespecialiseerde en eventuele fouten niet.

**Uploadfouten:**

**N<sup>1</sup>:** als het besturingssysteem Windows gegeneraliseerd en is geüpload als gespecialiseerd, krijgt u een time-outfout inrichten met de virtuele machine vastgelopen bij het OOBE-scherm.

**N<sup>2</sup>:** als het besturingssysteem is Windows, en is geüpload als gegeneraliseerd, wordt er een inrichten is mislukt met de virtuele machine op het scherm OOBE vastgelopen omdat de nieuwe virtuele machine wordt uitgevoerd met de naam van de oorspronkelijke computer, gebruikersnaam en wachtwoord.

**Resolutie**

U kunt beide deze fouten oplossen [Add-AzureRmVhd voor het uploaden van de oorspronkelijke VHD](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvhd), on-premises beschikbaar, met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerde/specifieke). Als u wilt uploaden als gegeneraliseerd, moet u eerst sysprep uitvoeren.

**Vastleggen fouten:**

**N<sup>3</sup>:** als het besturingssysteem Windows is gegeneraliseerd, en deze wordt vastgelegd als gespecialiseerd, krijgt u een inrichting time-outfout omdat de oorspronkelijke virtuele machine kan niet worden gebruikt omdat het is gemarkeerd als gegeneraliseerd.

**N<sup>4</sup>:** als het besturingssysteem is Windows, en deze wordt vastgelegd als gegeneraliseerd, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de naam van de oorspronkelijke computer, de gebruikersnaam en het wachtwoord. De oorspronkelijke virtuele machine is ook niet kan worden gebruikt omdat deze is gemarkeerd als gespecialiseerde.

**Resolutie**

Beide deze fouten oplossen, verwijdert u de huidige installatiekopie vanuit de portal en [opnieuw vanuit de huidige VHD vastleggen](../windows/create-vm-specialized.md) met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerde/specifieke).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probleem: Aangepaste/galerie/marketplace-installatiekopie; Toewijzingsfout
Deze fout zich voordoet in situaties als de nieuwe VM-aanvraag is vastgemaakt aan een cluster die op de VM-grootte wordt aangevraagd niet kan ondersteunen, of heeft geen beschikbare vrije ruimte voor de aanvraag.

**1 oorzaak:** het cluster kan niet de aangevraagde VM-grootte ondersteunt.

**Oplossing 1:**

* Probeer de aanvraag met een kleinere virtuele machine.
* Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
  * Stop alle virtuele machines in de beschikbaarheidsset.
    Klik op **resourcegroepen** > *uw resourcegroep* > **Resources** > *uw beschikbaarheidsset*  >  **Virtuele Machines** > *uw virtuele machine* > **stoppen**.
  * Nadat alle virtuele machines stoppen, moet u de nieuwe virtuele machine maken in de gewenste grootte.
  * Start de nieuwe virtuele machine eerst, en vervolgens selecteert u elk van de gestopte VM's en klik op **Start**.

**2 oorzaak:** het cluster heeft geen gratis resources.

**Oplossing 2:**

* De aanvraag opnieuw uitvoeren op een later tijdstip.
* Als de nieuwe virtuele machine deel van een andere beschikbaarheidsset uitmaken kan
  * Maak een nieuwe virtuele machine in een andere beschikbaarheidsset (in dezelfde regio bevinden).
  * De nieuwe virtuele machine toevoegen aan hetzelfde virtuele netwerk.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt wanneer u een gestopte virtuele machine van Windows starten of het formaat van een bestaande Windows-VM in Azure, Zie [problemen met opnieuw te starten of het formaat van een bestaande Windows-virtuele Machine in Azure Resource Manager oplossen implementatie](restart-resize-error-troubleshooting.md).


