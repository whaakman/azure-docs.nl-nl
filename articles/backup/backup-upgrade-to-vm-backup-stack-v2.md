---
title: Upgrade naar de Azure Resource Manager-implementatiemodel voor back-stack van virtuele machine in Azure | Microsoft Docs
description: Proces en veelgestelde vragen voor VM-back-stack, Resource Manager-implementatiemodel bijwerken
services: backup, virtual-machines
documentationcenter: ''
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: ''
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 03/08/2018
ms.author: trinadhk, sogup
ms.openlocfilehash: 224cd365e6b3ca4fd963b530dbaa289b763d53ee
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="upgrade-to-the-azure-resource-manager-deployment-model-for-azure-vm-backup-stack"></a>Een upgrade uitvoert naar de Azure Resource Manager-implementatiemodel voor back-stack van virtuele machine in Azure
Het implementatiemodel van Resource Manager voor de upgrade naar back-stack van virtuele machine (VM) biedt de volgende verbeteringen:
* De mogelijkheid om te zien van de momentopnamen die zijn gemaakt als onderdeel van een back-uptaak die beschikbaar is voor herstel zonder te wachten op voor de overdracht van gegevens te voltooien. De wachttijd voor momentopnamen kopiëren naar de kluis voordat restore vermindert. Deze mogelijkheid wordt ook voorkomen dat de vereiste extra opslag voor back-ups van VM's voor premium, met uitzondering van de eerste back-up.  

* Verlaging van de back-up en herstel tijden door momentopnamen lokaal gedurende zeven dagen bewaren.

* Ondersteuning voor de schijf de grootte van maximaal 4 TB.

* Mogelijkheid om het gebruik van een niet-beheerde VM oorspronkelijke storage-accounts bij het herstellen. Deze mogelijkheid bestaat zelfs wanneer de virtuele machine bevat schijven die over de storage-accounts zijn gedistribueerd. Deze zorgt herstelbewerkingen sneller voor een groot aantal verschillende configuraties van virtuele machine.
    > [!NOTE] 
    > Deze mogelijkheid is niet hetzelfde zijn als de oorspronkelijke virtuele machine te overschrijven. 
    >

## <a name="whats-changing-in-the-new-stack"></a>Wat wordt gewijzigd in de nieuwe stack?
Op dit moment wordt bestaat de back-uptaak uit twee fasen:
1.  Maken van een VM-momentopname. 
2.  Een VM-momentopname worden overgebracht naar de Azure Backup-kluis. 

Een herstelpunt wordt beschouwd als pas nadat fasen 1 en 2 hebt gemaakt. Als onderdeel van de nieuwe stack, wordt een herstelpunt gemaakt als de momentopname is voltooid. U kunt ook vanaf dit herstelpunt herstellen met behulp van dezelfde stroom terugzetten. U kunt dit herstelpunt in de Azure portal identificeren met behulp van 'snapshot' als het type van het herstel. Nadat de momentopname is overgebracht naar de kluis, het type van het herstel gewijzigd in 'momentopname en kluis'. 

![Back-uptaak in VM back-stack van Resource Manager-implementatiemodel--opslag en kluis](./media/backup-azure-vms/instant-rp-flow.jpg) 

Momentopnamen zijn standaard voor zeven dagen bewaard. Deze functie kunt het terugzetten van sneller voltooien van deze momentopnamen. Vermindert de tijd die nodig is om gegevens te kopiëren uit de kluis terug naar de storage-account van de klant. 

## <a name="considerations-before-upgrade"></a>Overwegingen voor de upgrade
* De upgrade van de VM-back-stack is één richting. Zodat alle back-ups werken in deze stroom. Omdat deze is ingeschakeld op het abonnementsniveau, gaat alle VM's in deze stroom. Alle nieuwe functie toevoegingen zijn gebaseerd op de dezelfde stack. Mogelijkheid om te bepalen die dit beleid niveau in de toekomst wordt binnenkort versies.

* Voor virtuele machines met premium-schijven, tijdens en tot de eerste back-up is voltooid, zorg ervoor dat er voldoende opslagruimte in de storage-account. Deze moet gelijk zijn aan de grootte van de virtuele machine.

* Momentopnamen worden lokaal opgeslagen herstelpunten vergroten en sneller herstellen. Daarom ziet u de opslagkosten die met momentopnamen binnen de periode zeven dagen overeenkomen.

* Incrementele momentopnamen worden opgeslagen als pagina-blobs. Alle klanten die gebruikmaken van niet-beheerde schijven voor de zeven dagen die de momentopnamen die zijn opgeslagen in het account van de lokale opslag van de klant in rekening worden gebracht. Volgens het actuele prijsmodel is er geen kosten voor klanten op beheerde schijven.

* Als u een herstel van een momentopname herstelpunt voor een premium VM doet, ziet u een locatie voor de tijdelijke opslag die wordt gebruikt tijdens het maken van de virtuele machine als onderdeel van het terugzetten.

* Voor premium storage-accounts nemen de momentopnamen die worden uitgevoerd voor chatberichten herstel 10 TB toegewezen ruimte.

## <a name="upgrade"></a>Upgraden
### <a name="the-azure-portal"></a>De Azure-portal
Als u de Azure-portal gebruikt, ziet u een melding op het kluisdashboard. Deze melding is gekoppeld aan back-up en herstel snelheid-verbeteringen en ondersteuning voor grote schijven.

![Back-uptaak in VM back-stack van Resource Manager-implementatiemodel--ondersteuning melding](./media/backup-azure-vms/instant-rp-banner.png) 

Als een scherm voor het upgraden naar de nieuwe stack, schakelt u de banner. 

![Back-uptaak in VM-back-stack Resource Manager-implementatiemodel--upgraden](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
Voer de volgende cmdlets uit vanaf een verhoogde PowerShell terminal:
1.  Meld u bij uw Azure-account: 

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Selecteer het abonnement dat u wilt registreren voor preview:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Dit abonnement voor private preview registreren:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Controleer of de upgrade is voltooid
Voer de volgende cmdlet vanaf een verhoogde PowerShell-terminal:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Als deze optie 'Geregistreerde', is uw abonnement bijgewerkt naar VM back-stack van Resource Manager-implementatiemodel.
