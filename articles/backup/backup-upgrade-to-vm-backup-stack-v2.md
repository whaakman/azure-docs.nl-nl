---
title: Een upgrade uitvoeren naar de Azure-VM-back-Upstack V2
description: Proces en veelgestelde vragen voor VM-back-upstack, Resource Manager-implementatiemodel bijwerken
services: backup
author: trinadhk
manager: vijayts
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 10/3/2018
ms.author: trinadhk
ms.openlocfilehash: 20c1606d4d6a1ddd43426731e5498d1bee47f2e3
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962530"
---
# <a name="upgrade-to-azure-vm-backup-stack-v2"></a>Upgrade uitvoeren naar Azure VM Backup-stack V2

Het Resource Manager-implementatiemodel voor de upgrade naar de virtuele machine (VM)-back-upstack biedt de volgende verbeteringen:

* De mogelijkheid om te zien van de momentopnamen die zijn gemaakt als onderdeel van een back-uptaak die is beschikbaar voor herstel zonder te wachten op voor de overdracht te voltooien. Beperkt het de wachttijd voor momentopnamen te kopiëren naar de kluis voordat het activeren van de herstelbewerking. Ook deze mogelijkheid wordt voorkomen dat de vereiste extra opslag voor back-ups van virtuele machines voor premium, met uitzondering van de eerste back-up.  

* Vermindert back-up en herstel met behoud van momentopnamen lokaal, zeven dagen.

* Ondersteuning voor de schijf de grootte van maximaal 4 TB.

* Mogelijkheid om een niet-beheerde VM oorspronkelijke storage-accounts kunt gebruiken bij het herstellen van. Deze mogelijkheid bestaat, zelfs wanneer de virtuele machine heeft schijven die zijn verdeeld over de storage-accounts. Het downloadproces versneld herstelbewerkingen voor een groot aantal VM-configuraties.
    > [!NOTE]
    > Deze mogelijkheid is niet hetzelfde als de schijven van de virtuele machine vervangen door gegevens van het herstelpunt.


## <a name="whats-changing-in-the-new-stack"></a>Wat wordt gewijzigd over de nieuwe stack?
Op dit moment bestaat de back-uptaak uit twee fasen:
1.  Maken van een VM-momentopname.
2.  Een momentopname van een virtuele machine worden overgebracht naar de Azure Backup-kluis.

Een herstelpunt wordt beschouwd als alleen nadat fasen 1 en 2 hebt gemaakt. Als onderdeel van de nieuwe stack, wordt een herstelpunt gemaakt als de momentopname is voltooid. U kunt ook vanaf dit herstelpunt herstellen met behulp van dezelfde stroom terugzetten. U kunt dit herstelpunt in Azure portal identificeren met behulp van 'snapshot' als het type herstelpunt. Nadat de momentopname worden overgedragen naar de kluis, het type herstelpunt gewijzigd in "momentopname en kluis."

![Back-uptaak in VM-back-upstack Resource Manager-implementatiemodel, opslag en de kluis](./media/backup-azure-vms/instant-rp-flow.jpg)

Momentopnamen zijn standaard voor zeven dagen bewaard. Deze functie kunt het terugzetten van sneller voltooien van deze momentopnamen. Dit vermindert de tijd die nodig is om gegevens te kopiëren van de kluis naar het storage-account van de klant.

## <a name="considerations-before-upgrade"></a>Overwegingen voor de upgrade

* De upgrade van de VM-back-upstack is een gerichte, alle back-ups met ingang van deze stroom. Omdat de wijziging uitgevoerd op het abonnementsniveau wordt, gaan alle virtuele machines in deze stroom. Alle nieuwe functie toevoegingen zijn gebaseerd op de dezelfde stack. U kunt niet op dit moment bepalen de stack op het beleidsniveau van.

* Momentopnamen worden lokaal opgeslagen om herstelpunten te verbeteren en om herstelbewerkingen te versnellen. Als gevolg hiervan, ziet u kosten voor opslag die overeenkomen met de momentopnamen die zijn gemaakt tijdens de periode van zeven dagen.

* Incrementele momentopnamen worden opgeslagen als pagina-blobs. Alle klanten die gebruikmaken van niet-beheerde schijven worden in rekening gebracht voor de zeven dagen die de momentopnamen worden opgeslagen in het account van de lokale opslag van de klant. Omdat de restore-punt verzamelingen die worden gebruikt door beheerde VM-back-ups blob-momentopnamen op het opslagniveau van de onderliggende, overeenkomt met de kosten voor beheerde schijven wordt Zie [blob-momentopname prijzen](https://docs.microsoft.com/rest/api/storageservices/understanding-how-snapshots-accrue-charges) en ze zijn.

* Als u een premium VM vanaf een herstelpunt momentopname herstellen, wordt een tijdelijke opslaglocatie gebruikt terwijl de virtuele machine wordt gemaakt.

* Voor premium storage-accounts de momentopnamen die voor instant recovery points aantal voor de limiet van 10 TB van toegewezen ruimte.

> [!NOTE]
> Een upgrade uitvoeren naar Azure VM Backup stack V2 voor ondersteuning van Azure Backup voor de [Standard-SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).

## <a name="upgrade"></a>Upgraden
### <a name="the-azure-portal"></a>Azure Portal
Als u de Azure-portal gebruikt, ziet u een melding op het kluisdashboard. Deze melding is gekoppeld aan ondersteuning voor grote schijven en verbeteringen in back-up en herstel snelheid. U kunt ook gaan naar de pagina eigenschappen van de kluis om op te halen van de upgrade-optie.

![Back-uptaak in VM-back-upstack Resource Manager-implementatiemodel--ondersteuning-melding](./media/backup-azure-vms/instant-rp-banner.png)

Als u wilt een scherm voor het upgraden naar de nieuwe stack openen, selecteer de banner.

![Back-uptaak in VM-back-upstack Resource Manager-implementatiemodel--upgrade](./media/backup-azure-vms/instant-rp.png)

### <a name="powershell"></a>PowerShell
De volgende cmdlets uitvoeren vanaf een hogergelegen PowerShell terminal:
1.  Meld u aan bij uw Azure-account:

    ```
    PS C:> Connect-AzureRmAccount
    ```

2.  Selecteer het abonnement dat u wilt registreren:

    ```
    PS C:>  Get-AzureRmSubscription –SubscriptionName "Subscription Name" | Select-AzureRmSubscription
    ```

3.  Dit abonnement registreren:

    ```
    PS C:>  Register-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="verify-that-the-upgrade-is-finished"></a>Controleer of de upgrade is voltooid
Voer de volgende cmdlet vanaf een verhoogde PowerShell terminal:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" –ProviderNamespace Microsoft.RecoveryServices
```

De status 'Registered', wordt vervolgens uw abonnement bijgewerkt naar VM-back-upstack Resource Manager-implementatiemodel.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

De volgende vragen en antwoorden zijn verzameld van forums en vragen van klanten.

### <a name="will-upgrading-to-v2-impact-current-backups"></a>Huidige back-ups invloed upgraden naar V2?
Als u een upgrade naar V2 uitvoert, is er niet van invloed op uw huidige back-ups en hoeft niet te configureren van uw omgeving. Upgrade en uw back-omgeving blijft werken als deze is.

### <a name="what-does-it-cost-to-upgrade-to-azure-vm-backup-stack-v2"></a>Wat zijn de kosten om te upgraden naar Azure VM Backup stack v2?
Er zijn geen kosten verbonden aan de stack upgraden naar v2. Momentopnamen worden lokaal opgeslagen voor het maken van het herstelpunt versnellen en herstelbewerkingen. Als gevolg hiervan, ziet u kosten voor opslag die met de momentopnamen die tijdens de periode van zeven dagen overeenkomen.

### <a name="does-upgrading-to-stack-v2-increase-the-premium-storage-account-snapshot-limit-by-10-tb"></a>Bijwerken voor stack v2 premium storage-accountlimiet voor de Momentopnames van 10 TB verhogen?
Nee, totale momentopnamelimiet per storage-account is nog steeds op 10TB.

### <a name="in-premium-storage-accounts-do-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>Premium-opslagaccounts kunt momentopnamen die zijn gemaakt voor directe herstelpunt dat de momentopname-limiet van 10 TB innemen?
Voor premium storage-accounts nemen de momentopnamen voor directe herstelpunt, Ja, de toegewezen 10 TB aan schijfruimte.

### <a name="how-does-the-snapshot-work-during-the-seven-day-period"></a>Hoe werkt de momentopname tijdens de periode van zeven dagen?
Elke dag een nieuwe momentopname wordt gemaakt. Er zijn zeven afzonderlijke momentopnamen. De service biedt **niet** nemen van een kopie op de eerste dag en het toevoegen van wijzigingen voor de komende zes dagen.

### <a name="is-a-v2-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Een v2-momentopname is een momentopname van incrementele of een volledige momentopname?
Incrementele momentopnamen worden gebruikt voor niet-beheerde schijven. Voor beheerde schijven, herstelpunt verzameling die zijn gemaakt door Azure Backup maakt gebruik van blob-momentopnamen en daarom worden incrementele.

### <a name="how-to-get-standard-ssd-managed-disk-support-for-a-virtual-machine"></a>Over het verkrijgen van standard-SSD managed disk-ondersteuning voor een virtuele machine?
Een upgrade uitvoeren naar Azure VM Backup stack V2 voor ondersteuning van Azure Backup voor de [Standard-SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/).
