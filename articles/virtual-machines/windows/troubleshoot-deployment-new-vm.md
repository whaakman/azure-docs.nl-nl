---
title: Problemen met implementatie van virtuele Windows-machine in Azure oplossen | Microsoft Docs
description: Resource Manager implementatieproblemen bij het maken van een nieuwe Windows virtuele machine in Azure oplossen
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fff29f6cfed4989386ca5bbd12184dce525add76
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a>Problemen bij de implementatie bij het maken van een nieuwe Windows VM in Azure oplossen
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

Zie voor andere VM-implementatieproblemen en vragen [problemen met implementatie Windows virtuele machine in Azure](troubleshoot-deploy-vm.md).

## <a name="collect-activity-logs"></a>Registreert activiteit verzamelen
U start het oplossen van problemen door de activiteitenlogboeken om u te identificeren van de fout die is gekoppeld aan het probleem te verzamelen. De volgende koppelingen bevatten gedetailleerde informatie over het proces te volgen.

[Implementatiebewerkingen bekijken](../../azure-resource-manager/resource-manager-deployment-operations.md)

[Activiteit-logboeken voor het beheren van Azure-resources bekijken](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** als het besturingssysteem Windows gegeneraliseerd, en deze wordt geüpload en/of met de algemene instelling vastgelegd, wordt niet eventuele fouten. Op dezelfde manier als het besturingssysteem Windows gespecialiseerde, en deze wordt geüpload en/of met de instelling voor gespecialiseerde vastgelegd en vervolgens niet eventuele fouten.

**Fouten uploaden:**

**N<sup>1</sup>:** als het besturingssysteem Windows gegeneraliseerd en is geüpload als gespecialiseerde, ontvangt u een time-outfout inrichten met de virtuele machine op het scherm OOBE vastgelopen.

**N<sup>2</sup>:** als het besturingssysteem is Windows gespecialiseerde en als gegeneraliseerd is geüpload, krijgt u een inrichting is mislukt met de virtuele machine op het scherm OOBE vastgelopen, omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Naamomzetting**

U kunt beide deze fouten oplossen [toevoegen-AzureRmVhd voor het uploaden van de oorspronkelijke VHD](https://msdn.microsoft.com/library/mt603554.aspx), beschikbare on-premises, met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke). Als u wilt uploaden als gegeneraliseerd, moet u eerst sysprep uitvoeren.

**Vastleggen fouten:**

**N<sup>3</sup>:** als het besturingssysteem Windows gegeneraliseerd en deze wordt vastgelegd als gespecialiseerde, ontvangt u een inrichting time-outfout omdat de oorspronkelijke virtuele machine kan niet worden gebruikt als deze is gemarkeerd als gegeneraliseerd.

**N<sup>4</sup>:** als het besturingssysteem is Windows gespecialiseerde en deze is vastgelegd, zoals gegeneraliseerd, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de naam van de oorspronkelijke computer, de gebruikersnaam en het wachtwoord. Ook de oorspronkelijke virtuele machine kan niet worden gebruikt omdat deze is gemarkeerd als gespecialiseerde.

**Naamomzetting**

Beide deze fouten oplossen, de huidige installatiekopie verwijderen vanuit de portal en [opnieuw vanaf het huidige VHD's vastleggen](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke).

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Probleem: Aangepaste/galerie/marketplace-installatiekopie; Toewijzingsfout
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
Als u problemen ondervindt wanneer u een gestopte virtuele machine van Windows te starten of het formaat van een bestaande Windows-machines in Azure, Zie [problemen bij de implementatie met opnieuw te starten of het formaat van een bestaande Windows virtuele Machine in Azure Resource Manager oplossen](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

