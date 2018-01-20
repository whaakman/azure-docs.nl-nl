---
title: Linux-VM-implementatie klassieke oplossen | Microsoft Docs
description: Klassieke implementatieproblemen bij het maken van een nieuwe virtuele Linux-machine in Azure oplossen
services: virtual-machines-linux
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: c8a963fa-6b2a-4c7a-a1f4-7793adb02b19
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/06/2016
ms.author: cjiang
ms.openlocfilehash: 581fbaa477bd603fea5fdc0ef77c6ef7498b7897
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Klassieke implementatieproblemen met het maken van een nieuwe virtuele Linux-machine in Azure oplossen
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor de Resource Manager-versie van dit artikel, [hier](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>De auditlogboeken verzamelen
U start het oplossen van problemen door de controlelogboeken om te identificeren van de fout die is gekoppeld aan het probleem te verzamelen.

Klik in de Azure-portal op **Bladeren** > **virtuele machines** > *uw Windows-machine* > **instellingen** > **controlelogboeken**.

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** als het besturingssysteem Linux gegeneraliseerd, en deze wordt geüpload en/of met de algemene instelling vastgelegd, wordt niet eventuele fouten. Op dezelfde manier als het besturingssysteem zich Linux gespecialiseerde, en deze wordt geüpload en/of met de instelling voor gespecialiseerde vastgelegd en vervolgens niet eventuele fouten.

**Fouten uploaden:**

**N<sup>1</sup>:** als het besturingssysteem is gegeneraliseerd Linux, en is geüpload als gespecialiseerde, ontvangt u een inrichting time-outfout omdat de virtuele machine in de inrichting stadium is vastgelopen.

**N<sup>2</sup>:** als het besturingssysteem is Linux gespecialiseerde en als gegeneraliseerd is geüpload, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord.

**Oplossing:**

Als u wilt zowel deze fouten oplossen, de oorspronkelijke schijf geüpload, beschikbare on-premises, met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke). Als u wilt uploaden als gegeneraliseerd, vergeet niet om uit te voeren - inrichting eerst ervan ongedaan maakt. Zie [maken en uploaden van een virtuele harde schijf met het Linux-besturingssysteem](create-upload-vhd-classic.md) voor meer informatie.

**Vastleggen fouten:**

**N<sup>3</sup>:** als het besturingssysteem gegeneraliseerd Linux is, en deze wordt vastgelegd als gespecialiseerde, ontvangt u een inrichting time-outfout omdat de oorspronkelijke virtuele machine kan niet worden gebruikt als deze is gemarkeerd als gegeneraliseerd.

**N<sup>4</sup>:** als het besturingssysteem is Linux gespecialiseerde en deze is vastgelegd, zoals gegeneraliseerd, krijgt u een inrichting is mislukt omdat de nieuwe virtuele machine wordt uitgevoerd met de oorspronkelijke computernaam, gebruikersnaam en wachtwoord. Ook de oorspronkelijke virtuele machine kan niet worden gebruikt omdat deze is gemarkeerd als gespecialiseerde.

**Oplossing:**

Beide deze fouten oplossen, de huidige installatiekopie verwijderen vanuit de portal en [opnieuw vanaf het huidige VHD's vastleggen](capture-image-classic.md) met dezelfde instelling als die voor het besturingssysteem (gegeneraliseerd/specifieke).

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
Als u problemen ondervindt wanneer u een gestopte Linux VM starten of het formaat van een bestaande Linux VM in Azure, Zie [klassieke implementatieproblemen oplossen met opnieuw te starten of het formaat van een bestaande virtuele Linux-Machine in Azure](restart-resize-error-troubleshooting.md).

