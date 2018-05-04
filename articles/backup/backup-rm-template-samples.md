---
title: Azure Resource Manager-sjablonen voor Azure Backup | Microsoft Docs
description: Azure Backup PowerShell-voorbeelden
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
tags: ''
ms.assetid: ''
ms.service: backup
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 04/18/2018
ms.author: markgal
ms.custom: mvc
ms.openlocfilehash: b8502e4e3934b36fb4c8ccac00f4fa14565780d9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="azure-resource-manager-templates-for-azure-backup"></a>Azure Resource Manager-sjablonen voor Azure Backup

De volgende tabel bevat koppelingen naar Azure Resource Manager-sjablonen voor gebruik bij Recovery Services-kluizen en Azure Backup-functies.

|   |   |
|---|---|
|**Recovery Services-kluis** | |
| [Een Recovery Services-kluis maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vault-create)| Maak een Recovery Services-kluis. De kluis kan worden gebruikt voor Azure Backup en Azure Site Recovery. |
|**Back-up maken van virtuele machines**| |
| [Back-ups maken van Resource Manager-VM's](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-backup-vms) | Gebruik de bestaande Recovery Services-kluis en het back-upbeleid om back-ups te maken van Resource Manager-virtuele machines in dezelfde resourcegroep.|
| [Back-ups maken van IaaS-VM's naar een Recovery Services-kluis](https://github.com/Azure/azure-quickstart-templates/tree/master/201-recovery-services-backup-classic-resource-manager-vms) | Sjabloon voor het maken van back-ups van klassieke en Resource Manager-virtuele machines. |
| [Beleid voor wekelijkse back-up voor IaaS VM's maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-weekly-backup-policy-create) | Met de sjabloon wordt een Recovery Services-kluis en een beleid voor wekelijkse back-ups gemaakt dat wordt gebruikt voor het maken van back-ups van klassieke en Resource Manager-virtuele machines.|
| [Beleid voor dagelijkse back-up voor IaaS VM's maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-daily-backup-policy-create) | Met de sjabloon wordt een Recovery Services-kluis en een beleid voor dagelijkse back-ups gemaakt dat wordt gebruikt voor het maken van back-ups van klassieke en Resource Manager-virtuele machines.|
| [Windows Server-VM met back-up implementeren](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-create-vm-and-configure-backup) | Met de sjabloon wordt een Windows Server-VM en Recovery Services-kluis gemaakt met het standaard back-upbeleid ingeschakeld.|
|**Back-uptaken controleren** |  |
| [OMS Log Analytics gebruiken voor het controleren van Azure Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-backup-oms-monitoring) | Met de sjabloon wordt OMS-bewaking voor Azure Backup geïmplementeerd, waarmee u back-up- en hersteltaken, back-upwaarschuwingen en de cloudopslag in uw Recovery Services-kluizen kunt controleren.|  
|   |   |

