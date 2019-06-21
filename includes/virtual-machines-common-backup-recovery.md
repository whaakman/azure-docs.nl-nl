---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: be71b269e618d13a126e4005754b307e9c6517d7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176029"
---
## <a name="azure-backup"></a>Azure Backup

Voor back-ups van virtuele Azure-machines waarop werkbelastingen voor productie wordt uitgevoerd, gebruikt u Azure Backup. Azure Backup biedt ondersteuning voor toepassingsconsistente back-ups voor zowel Windows als Linux-VM's. Azure Backup maakt herstelpunten die worden opgeslagen in geografisch redundante Recovery Services-kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele VM of alleen specifieke bestanden herstellen. 

Voor een eenvoudige introductie tot Azure back-up voor Azure-VM's, Zie de zelfstudie 'Back-up van virtuele machines van Azure' voor [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) of [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).

Zie voor meer informatie over hoe Azure Backup werkt [plannen van uw VM-back-upinfrastructuur in Azure](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery beveiligt uw VM's uit een grote ramp-scenario, wanneer een hele regio optreedt in een uitval vanwege de grote natuurramp of wijdverbreid service wordt onderbroken. U kunt Azure Site Recovery voor uw VM's configureren, zodat u uw toepassing met één klik in een paar minuten kunt herstellen. U kunt repliceren naar een Azure-regio van uw keuze, het is niet beperkt tot twee gekoppelde regio's. 

U kunt herstel na noodgevallen oefeningen uitvoeren met testfailovers op aanvraag, zonder gevolgen voor uw productieworkloads of de voortdurende replicatie. Plannen voor herstel voor het indelen van failover en failback van de gehele toepassing die wordt uitgevoerd op meerdere virtuele machines maken. De functie voor herstel plan is geïntegreerd met Azure automation-runbooks.

U kunt aan de slag door [repliceren van uw virtuele machines](https://aka.ms/a2a-getting-started). 

## <a name="managed-snapshots"></a>Beheerde momentopnamen 

Een optie snel en eenvoudig back-ups van virtuele machines die gebruikmaken van beheerde schijven in de ontwikkel- en testomgevingen bieden aan momentopnamen. Een beheerde momentopname is een alleen-lezen kopie van een beheerde schijf. Momentopnamen onafhankelijk van de bronschijf bestaan en kunnen worden gebruikt voor het maken van nieuwe beheerde schijven voor het opnieuw opbouwen van een virtuele machine. Ze worden in rekening gebracht op basis van het gebruikte deel van de schijf. Als u bijvoorbeeld een momentopname maakt van een beheerde schijf met een ingerichte capaciteit van 64 GB terwijl de werkelijke gebruikte gegevensgrootte 10 GB is, wordt de momentopname alleen gefactureerd voor de gebruikte gegevensgrootte van 10 GB.  

Zie voor meer informatie over het maken van momentopnamen.:

* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Volgende stappen
U kunt Azure Backup aan de hand van de 'Back-up Windows virtuele machines zelfstudie' uitproberen voor [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) of [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).
