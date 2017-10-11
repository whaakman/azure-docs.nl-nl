---
title: Community-tools - klassieke resources verplaatsen naar Azure Resource Manager | Microsoft Docs
description: In dit artikel catalogus maken van de hulpprogramma's die zijn opgegeven door de community om te migreren van IaaS-middelen van klassiek naar de Azure Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: d3fc0246088eddeb345bea0ffbd2c5247b218006
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="community-tools-to-migrate-iaas-resources-from-classic-to-azure-resource-manager"></a>Community-programma’s voor de migratie van IaaS-resources van klassiek naar Azure Resource Manager
In dit artikel catalogus maken van de hulpprogramma's die zijn opgegeven door de community om te helpen bij de migratie van IaaS-middelen van klassiek naar de Azure Resource Manager-implementatiemodel.

> [!NOTE]
> Deze hulpprogramma's zijn niet officieel ondersteund door Microsoft Support. Daarom zijn open source op GitHub en we willen graag PRs accepteren voor oplossingen of aanvullende scenario's. Om een probleem melden, gebruikt u de functie van de problemen met GitHub.
> 
> Met deze hulpprogramma's voor migratie, zullen uitvaltijd voor uw klassieke virtuele Machine. Als u de migratie ondersteund platform zoekt, gaat u naar 
> 
>   * [Migratie van IaaS-middelen van klassiek naar Azure Resource Manager-stack ondersteund platform](migration-classic-resource-manager-overview.md)
>   * [Technische Deep Dive op Platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md)
>   * [Migreren van IaaS-middelen van klassiek naar Azure Resource Manager met Azure PowerShell](migration-classic-resource-manager-ps.md)
> 
> 

## <a name="asmmetadataparser"></a>AsmMetadataParser
Dit is een verzameling hulpprogramma's die zijn gemaakt als onderdeel van enterprise-migraties van Azure Service Management in Azure Resource Manager. Dit hulpprogramma kunt u uw infrastructuur repliceren naar een ander abonnement die kan worden gebruikt voor het testen van migratie en ijzer uit eventuele problemen voordat u de migratie op uw productie-abonnement.

[Koppeling naar de documentatie van het hulpprogramma](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/AsmToArmMigrationApiToolset)

## <a name="migaz"></a>migAz
migAz is een extra optie voor een volledige reeks klassieke IaaS-bronnen migreren naar Azure Resource Manager IaaS-middelen. De migratie kan zich voordoen binnen hetzelfde abonnement of tussen verschillende abonnementen en typen abonnementen (ex: CSP-abonnementen).

[Koppeling naar de documentatie van het hulpprogramma](https://github.com/Azure/migAz)

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van de migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Lees de veelgestelde vragen over IaaS Resourcemigratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

