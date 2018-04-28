---
title: Upgrade uitvoeren naar Azure VM-back-stack V2 | Microsoft Docs
description: Proces en veelgestelde vragen voor VM-back-stack V2 bijwerken
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
ms.openlocfilehash: 7e092dc1448a45277e01b1a8c6d2bc0e2a8a22a3
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="upgrade-to-vm-backup-stack-v2"></a>Een upgrade uitvoeren naar VM-back-up-stack V2
De upgrade van de back-stack V2 virtuele machine (VM) biedt de volgende verbeteringen:
* De mogelijkheid om te zien van de momentopname die is genomen als onderdeel van de back-uptaak alleen beschikbaar voor herstel zonder te wachten op voor de overdracht van gegevens om te voltooien.
Dit verkort de wachttijd op momentopname moet worden gekopieerd als u wilt voordat restore-kluis. Hierdoor vermindert bovendien de vereiste extra opslag voor back-ups van VM's voor premium, met uitzondering van eerste back-up.  

* Verlaging van de back-up en herstel tijden door momentopnamen lokaal gedurende 7 dagen bewaren. 

* Ondersteuning voor de schijf de grootte van maximaal 4 TB.  

* Oorspronkelijke storage-accounts (zelfs als de virtuele machine heeft de schijven die zijn verdeeld over de storage-accounts) wanneer gebruiken tijdens het doorzoeken van een herstel van een niet-beheerde virtuele machine. Hierdoor werkt herstelacties sneller voor een groot aantal verschillende configuraties van virtuele machine. 
    > [!NOTE] 
    > Dit is niet hetzelfde als de oorspronkelijke virtuele machine te overschrijven. 
    > 
    >

## <a name="what-is-changing-in-the-new-stack"></a>Wat is gewijzigd in de nieuwe stack?
Vandaag de dag is bestaat de back-uptaak uit twee fasen:
1.  VM momentopname te maken. 
2.  VM-momentopname worden overgebracht naar Azure Backup-kluis. 

Een herstelpunt wordt beschouwd als pas nadat fasen 1 en 2 hebt gemaakt. Als onderdeel van de nieuwe stack, wordt een herstelpunt gemaakt als de momentopname is voltooid. U kunt ook herstellen vanaf dit herstelpunt met behulp van dezelfde stroom terugzetten. U kunt dit herstelpunt in de Azure portal met 'momentopname' als punt hersteltype identificeren. Als de momentopname is overgebracht naar de kluis, wordt punt hersteltype gewijzigd in 'Snapshot kluis'. 

![Back-uptaak in VM-back-stack V2](./media/backup-azure-vms/instant-rp-flow.jpg) 

Standaard wordt momentopnamen worden bewaard gedurende zeven dagen. Hierdoor kan het terugzetten van sneller worden uitgevoerd vanaf deze momentopnamen doordat de tijd die nodig zijn om gegevens te kopiëren uit de kluis terug naar de klant storage-account. 

## <a name="considerations-before-upgrade"></a>Overwegingen voor de upgrade
* Dit is een upgrade van één richting van de VM-back-stack. Ja, gaat alle toekomstige back-ups in deze stroom. Aangezien **is ingeschakeld op het abonnementsniveau van een, gaan alle virtuele machines op deze stroom**. Alle nieuwe functie toevoegingen wordt gebaseerd op de dezelfde stack. Mogelijkheid om te bepalen die dit beleid niveau in de toekomst wordt binnenkort versies. 
* Virtuele machines met premium-schijven, tijdens de eerste back-up Zorg ervoor dat gelijk is aan de grootte van de virtuele machine opslagruimte beschikbaar in het opslagaccount is totdat de eerste back-up is voltooid. 
* Omdat momentopnamen lokaal opgeslagen worden vergroten herstelpunt maken en terugzetten versnellen, ziet u de opslagkosten die overeenkomt met momentopnamen binnen de periode zeven dagen.
* Incrementele momentopnamen worden opgeslagen als pagina-blobs. Alle de klanten die gebruikmaken van niet-beheerde schijven gefactureerd voor de zeven dagen momentopnamen opgeslagen in het account van de lokale opslag van de klant. Aan de hand van het actuele prijsmodel is er geen kosten voor klanten op beheerde schijven.
* Als u een terugzetbewerking in momentopname herstelpunt voor een VM voor Premium doet, ziet u een locatie voor de tijdelijke opslag worden gebruikt terwijl de virtuele machine wordt gemaakt als onderdeel van het terugzetten. 
* In geval van premium storage-accounts, wordt de momentopnamen die genomen werden voor direct herstellen beslaan 10 TB ruimte toegewezen in de premium-opslagaccount.

## <a name="how-to-upgrade"></a>Het bijwerken?
### <a name="the-azure-portal"></a>De Azure-portal
Als u met de Azure portal gebruikt, wordt u een melding weergegeven op het kluisdashboard die verband houden met ondersteuning voor grote schijven en back-up en herstel verbeteringen snelheid.

![Back-uptaak in VM-back-stack V2](./media/backup-azure-vms/instant-rp-banner.png) 

Als een scherm voor het upgraden naar de nieuwe stack, schakelt u de banner. 

![Back-uptaak in VM-back-stack V2](./media/backup-azure-vms/instant-rp.png) 

### <a name="powershell"></a>PowerShell
De volgende cmdlets uitvoeren vanuit een verhoogde PowerShell terminal:
1.  Aanmelden bij uw Azure-account. 

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

## <a name="verify-whether-the-upgrade-is-complete"></a>Controleer of de upgrade voltooid is
Voer de volgende cmdlet vanaf een verhoogde PowerShell-terminal:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

Als geregistreerd wordt, is klikt u vervolgens uw abonnement bijgewerkt naar VM-back-stack V2. 



