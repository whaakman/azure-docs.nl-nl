---
title: Community-hulpprogramma's - klassieke resources te verplaatsen naar Azure Resource Manager | Microsoft Docs
description: Dit artikel behandelen we de hulpprogramma's die zijn verleend door de community om u te helpen bij het migreren van IaaS-resources van klassieke naar het Azure Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 085a0ff0eee260069d693b339521a9489df78e40
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60848228"
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>Community-programma’s voor de migratie van IaaS-resources van klassiek naar Azure Resource Manager
Dit artikel behandelen we de hulpprogramma's die zijn verleend door de community om te helpen bij de migratie van IaaS-resources van klassiek naar de Azure Resource Manager-implementatiemodel.

> [!NOTE]
> Deze hulpprogramma's zijn niet officieel ondersteund door Microsoft Support. Daarom worden ze zijn open source op GitHub en we zijn blij te accepteren van pull-aanvragen voor correcties of aanvullende scenario's. Als u een probleem wilt melden, gebruik de functie voor GitHub-problemen.
> 
> Migreren met deze hulpprogramma's, wordt de downtime veroorzaken voor uw klassieke virtuele Machine. Als u naar ondersteunde platformmigratie zoekt, gaat u naar 
> 
>   * [Platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager-stack](migration-classic-resource-manager-overview.md)
>   * [Technische details van het Platform ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md)
>   * [Migratie van IaaS-resources van klassiek naar Azure Resource Manager met behulp van Azure PowerShell](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Dit is een verzameling hulpprogramma's die zijn gemaakt als onderdeel van enterprise-migraties van Azure Service Management naar Azure Resource Manager. Dit hulpprogramma kunt u uw infrastructuur repliceren naar een ander abonnement die kan worden gebruikt voor het testen van migratie en ijzer eventuele problemen voordat u de migratie uitvoert op uw productie-abonnement.

[Koppeling naar het hulpprogramma voor](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz is een extra optie voor een volledige set klassieke IaaS-resources migreren naar Azure Resource Manager IaaS-resources. De migratie kan zich voordoen binnen hetzelfde abonnement of tussen verschillende abonnementen en abonnementstypen (ex: CSP-abonnementen).

[Koppeling naar het hulpprogramma voor](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technische details over door platforms ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Lees de veelgestelde vragen over migratie IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

