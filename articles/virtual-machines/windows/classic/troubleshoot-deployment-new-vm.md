---
title: Virtuele machine van Windows deployment-klassieke oplossen | Microsoft Docs
description: Klassieke implementatieproblemen bij het maken van een nieuwe Windows virtuele machine in Azure oplossen
services: virtual-machines-windows
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 9f01d237-ba39-4c32-b72d-18f5f505d43a
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 5a1ad33bd8c287910458d26a96858e85c689fb16
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Klassieke implementatieproblemen met het maken van een nieuwe Windows virtuele machine in Azure oplossen
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor de Resource Manager-versie van dit artikel, [hier](../../virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>De auditlogboeken verzamelen
U start het oplossen van problemen door de controlelogboeken om te identificeren van de fout die is gekoppeld aan het probleem te verzamelen.

Klik in de Azure-portal op **Bladeren** > **virtuele machines** > *uw Windows-machine* > **instellingen** > **controlelogboeken**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** als het besturingssysteem Windows gegeneraliseerd, en deze wordt geüpload en/of met de algemene instelling vastgelegd, wordt niet eventuele fouten. Op dezelfde manier als het besturingssysteem Windows gespecialiseerde, en deze wordt geüpload en/of met de instelling voor gespecialiseerde vastgelegd en vervolgens niet eventuele fouten.

**Fouten uploaden:**

**N<sup>1</sup>:** als het besturingssysteem Windows gegeneraliseerd en is geüpload als gespecialiseerde, ontvangt u een time-outfout inrichten met de virtuele machine op het scherm OOBE vastgelopen.

**N<sup>2</sup>:** als het besturingssysteem is Windows gespecialiseerde en als gegeneraliseerd is geüpload, krijgt u een inrichting is mislukt met de virtuele machine op het scherm OOBE vastgelopen, omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Oplossing:**

Als u wilt zowel deze fouten oplossen, de oorspronkelijke schijf geüpload, beschikbare on-premises, met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke). Als u wilt uploaden als gegeneraliseerd, moet u eerst sysprep uitvoeren. Zie [een Windows Server-VHD naar Azure maken en uploaden](createupload-vhd.md) voor meer informatie.

**Vastleggen fouten:**

**N<sup>3</sup>:** als het besturingssysteem Windows gegeneraliseerd en deze wordt vastgelegd als gespecialiseerde, ontvangt u een inrichting time-outfout omdat de oorspronkelijke virtuele machine kan niet worden gebruikt als deze is gemarkeerd als gegeneraliseerd.

**N<sup>4</sup>:** als het besturingssysteem is Windows gespecialiseerde en deze is vastgelegd, zoals gegeneraliseerd, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord. Ook de oorspronkelijke virtuele machine kan niet worden gebruikt omdat deze is gemarkeerd als gespecialiseerde.

**Oplossing:**

Beide deze fouten oplossen, de huidige installatiekopie verwijderen vanuit de portal en [opnieuw vanaf het huidige VHD's vastleggen](capture-image.md) met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke).

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Probleem: Aangepaste / galerie / marketplace-installatiekopie; Toewijzingsfout
Deze fout zich voordoet in situaties wanneer de nieuwe VM-aanvraag wordt verzonden naar een cluster heeft geen beschikbare vrije ruimte voor de aanvraag of de aangevraagde VM-grootte kan niet worden ondersteund. Het is niet mogelijk om te combineren andere reeks van virtuele machines in dezelfde cloudservice. Dus als u maken van een nieuwe virtuele machine van een ander formaat wilt dan wat uw cloudservice kan ondersteunen, mislukt de compute-aanvraag.

Er kunnen een fout is veroorzaakt door een van twee situaties optreden, afhankelijk van de beperkingen van de cloudservice die u kunt de nieuwe virtuele machine maken.

**1 oorzaak:** de cloudservice is vastgemaakt aan een specifiek cluster of het bestand is gekoppeld aan een affiniteitsgroep en daarom is vastgemaakt aan een specifiek cluster aan het ontwerp. Dus nieuwe compute resource aanvragen in de betreffende affiniteitsgroep bevinden worden geprobeerd in hetzelfde cluster waar de bestaande bronnen worden gehost. Echter hetzelfde cluster mogelijk geen ondersteuning voor de aangevraagde VM-grootte of hebt onvoldoende beschikbare ruimte, wat resulteert in een toewijzingsfout. Dit geldt of de nieuwe resources zijn gemaakt via een nieuwe cloudservice of een bestaande cloudservice.

**Oplossing 1:**

* Een nieuwe cloudservice maken en deze koppelen aan een regio of een regio op basis van een virtueel netwerk.
* Maak een nieuwe virtuele machine in de nieuwe cloudservice.
  Als u een fout opgetreden krijgt bij het maken van een nieuwe cloudservice, probeer op een later tijdstip of de regio voor de cloudservice wijzigen.

> [!IMPORTANT]
> Als u probeerde te maken van een nieuwe virtuele machine in een bestaande cloudservice, maar kan niet en moest het maken van een nieuwe cloudservice voor uw nieuwe virtuele machine, kunt u alle virtuele machines in dezelfde cloudservice consolideren. Verwijderen van de virtuele machines in de bestaande cloudservice om dit te doen, en ze opnieuw vastleggen van de schijven in de nieuwe cloudservice. Het is echter belangrijk dat u weet dat de nieuwe cloudservice heeft een nieuwe naam en het VIP, dus moet u deze voor de afhankelijkheden die momenteel gebruikmaken van deze informatie voor de bestaande cloudservice bijwerken.
> 
> 

**2 oorzaak:** de cloudservice is gekoppeld aan een virtueel netwerk dat is gekoppeld aan een affiniteitsgroep, zodat deze naar een specifieke cluster is vastgemaakt aan het ontwerp. Alle nieuwe compute resource aanvragen in de betreffende affiniteitsgroep bevinden worden daarom geprobeerd in hetzelfde cluster waar de bestaande bronnen worden gehost. Echter hetzelfde cluster mogelijk geen ondersteuning voor de aangevraagde VM-grootte of hebt onvoldoende beschikbare ruimte, wat resulteert in een toewijzingsfout. Dit geldt of de nieuwe resources zijn gemaakt via een nieuwe cloudservice of een bestaande cloudservice.

**Oplossing 2:**

* Maak een nieuw regionaal virtueel netwerk.
* De nieuwe virtuele machine maken in het nieuwe virtuele netwerk.
* [Verbinding maken met uw bestaande virtuele netwerk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) naar het nieuwe virtuele netwerk. Zie voor meer informatie over [regionale virtuele netwerken](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). U kunt ook [uw virtueel netwerk voor op basis van affiniteit-groep migreren naar een regionaal virtueel netwerk](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), en maak vervolgens de nieuwe virtuele machine.

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt wanneer u een gestopte virtuele machine van Windows te starten of het formaat van een bestaande Windows-machines in Azure, Zie [klassieke implementatieproblemen oplossen met opnieuw te starten of het formaat van een bestaande Windows virtuele Machine in Azure](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md).

