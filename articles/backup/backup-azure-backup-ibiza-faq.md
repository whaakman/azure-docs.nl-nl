---
title: Veelgestelde vragen over Recovery Services-kluis | Microsoft Docs
description: In deze versie van de veelgestelde vragen wordt de openbare evaluatieversie van de Azure Backup-service behandeld. Antwoorden op veelgestelde vragen over Backup-agent, het maken, bewaren, terugzetten en beveiligen van back-ups en andere veelgestelde vragen over Azure Backup.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-upoplossing; Backup-service
ms.assetid: 5f55b500-1ee9-4f64-9306-02d6f7a8eded
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: markgal;trinadhk;
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: e5ef305d926a57e32cdebd44f3dbe2185c735dd4
ms.lasthandoff: 04/15/2017


---
# <a name="recovery-services-vault---faq"></a>Recovery Services-kluis - veelgestelde vragen
Dit artikel bevat informatie over de Recovery Services-kluis en vormt een aanvulling op [Veelgestelde vragen over Azure Backup](backup-azure-backup-faq.md). Veelgestelde vragen over Azure Backup bevat de volledige serie vragen en antwoorden over de Azure Backup-service.  

U kunt vragen over Azure Backup stellen in de sectie Disqus van dit artikel of in een verwant artikel. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Recovery Services-kluizen zijn op basis van Resource Manager. Worden Backup-kluizen (klassieke modus) nog steeds ondersteund? <br/>
Ja, Backup-kluizen worden nog steeds ondersteund. U maakt Backup-kluizen in de [klassieke portal](https://manage.windowsazure.com). Recovery Services-kluizen worden gemaakt in de [Azure Portal](https://portal.azure.com). We raden u echter aan Recovery Services-kluizen te maken, aangezien alle toekomstige verbeteringen alleen beschikbaar zullen zijn in Recovery Services-kluizen.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Kan ik een Backup-kluis migreren naar een Recovery Services-kluis? <br/>
Op dit moment is het niet mogelijk om de inhoud van een Backup-kluis te migreren naar een Recovery Services-kluis. We werken ernaar toe om deze functionaliteit toe te voegen, maar dit is niet beschikbaar in de openbare evaluatieversie.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Ondersteunen Recovery Services-kluizen klassieke virtuele machines of virtuele machines op basis van Resource Manager? <br/>
Recovery Services-kluizen ondersteunen beide modellen.  In een Recovery Services-kluis kunt u een back-up opslaan van een VM die is gemaakt in de klassieke portal (VM’s in de klassieke modus) of een back-up van een VM die is gemaakt in de Azure Portal (op basis van Resource Manager).

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Ik heb back-ups van mijn klassieke VM’s opgeslagen in de back-upkluis. Nu wil ik mijn VM’s migreren van de klassieke modus naar de Resource Manager-modus.  Hoe kan ik een back-up ervan opslaan in de Recovery Services-kluis?
Back-ups van klassieke virtuele machines in de back-upkluis worden niet automatisch naar de Recovery Services-kluis gemigreerd wanneer u de virtuele machines van de klassieke naar de Resource Manager-modus migreert. Volg deze stappen voor migratie van back-ups van VM’s:

1. Ga in de back-upkluis naar het tabblad **Beveiligde Items** en selecteer de virtuele machine. Klik op [Beveiliging stoppen](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Laat de optie *Gekoppelde back-upgegevens verwijderen* **uitgeschakeld**.
2. Ga in [Azure Portal](https://portal.azure.com) naar het menu **Extensies** voor de virtuele machine en verwijder de extensie **VMSnapshot/VMSnapshotLinux**.
3. Migreer de virtuele machines van de klassieke modus naar de Resource Manager-modus. Zorg ervoor dat de opslagruimte en het netwerk die corresponderen met de virtuele machine ook naar de Resource Manager modus worden gemigreerd.
4. Maak een Recovery Services-kluis en configureer de back-up op de gemigreerde virtuele machine met behulp van de actie **Back-up** bovenaan in het dashboard van de kluis. Meer informatie over het [inschakelen van back-ups in de Recovery Services-kluis](backup-azure-vms-first-look-arm.md)

