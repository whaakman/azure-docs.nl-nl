---
title: Gebruikmaken van Azure direct herstel
description: Azure direct herstellen mogelijkheden en veelgestelde vragen over de virtuele machine back-up-stack, Resource Manager-implementatiemodel
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: sogup
ms.openlocfilehash: 566faaef4e91237bd0a0f915ea39f4bd389120d8
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593077"
---
# <a name="get-improved-backup-and-restore-performance-with-azure-backup-instant-restore-capability"></a>Ophalen van verbeterde back-up en herstellen van de prestaties met mogelijkheid Azure back-up direct herstellen

> [!NOTE]
> Op basis van feedback van gebruikers naam **VM-back-upstack V2** naar **direct herstellen** aan de voorkoming van verwarring met Azure Stack-functionaliteit.

Het nieuwe model voor direct terugzetten biedt de volgende verbeteringen:

* De mogelijkheid om te gebruiken van momentopnamen die zijn gemaakt als onderdeel van een back-uptaak die is beschikbaar voor herstel zonder te wachten op voor de overdracht van gegevens naar de kluis om te voltooien. Beperkt het de wachttijd voor momentopnamen te kopiëren naar de kluis voordat het activeren van de herstelbewerking.
* Vermindert back-up en herstel met behoud van momentopnamen lokaal voor standaard twee dagen. Deze kluis standaard worden geconfigureerd op een waarde tussen 1 tot 5 dagen.
* Ondersteunt schijf de grootte van maximaal 4 TB.
* Standard-SSD-schijven ondersteunt.
*   Mogelijkheid om te gebruiken van een niet-beheerde virtuele machine oorspronkelijk opslagaccounts (per schijf), bij het herstellen. Deze mogelijkheid bestaat, zelfs wanneer de virtuele machine heeft schijven die zijn verdeeld over de storage-accounts. Het downloadproces versneld herstelbewerkingen voor een groot aantal VM-configuraties



## <a name="whats-new-in-this-feature"></a>Wat is er nieuw in deze functie

Op dit moment bestaat de back-uptaak uit twee fasen:

1.  Maken van een VM-momentopname.
2.  Een momentopname van een virtuele machine worden overgebracht naar de Azure Recovery Services-kluis.

Een herstelpunt wordt beschouwd als gemaakt nadat fasen 1 en 2 zijn voltooid. Als onderdeel van deze upgrade, wordt een herstelpunt gemaakt als u de momentopname is voltooid en dit herstelpunt van het type van de momentopname kan worden gebruikt om uit te voeren van een herstelpunt met behulp van dezelfde stroom terugzetten. U kunt dit herstelpunt in Azure portal met behulp van 'snapshot' als het type herstelpunt identificeren en nadat de momentopname worden overgedragen naar de kluis, het type herstelpunt gewijzigd in 'momentopname en kluis'.

![Back-uptaak in VM-back-upstack Resource Manager-implementatiemodel, opslag en de kluis](./media/backup-azure-vms/instant-rp-flow.png)

Momentopnamen worden standaard gedurende twee dagen bewaard. Deze functie kunt er met deze momentopnamen herstelbewerkingen bewerking te knippen uitvaltijden het terugzetten. Dit vermindert de tijd die nodig is om te transformeren en gegevens van de kluis naar storage-account van de gebruiker voor niet-beheerde schijf-scenario's voor gebruikers van de beheerde schijf kopiëren, worden er beheerde schijven te maken van de Recovery Services-gegevens gemaakt.

## <a name="feature-considerations"></a>Functie-overwegingen

* Momentopnamen worden opgeslagen samen met de schijven om herstelpunten te verbeteren en om herstelbewerkingen te versnellen. Als gevolg hiervan, ziet u kosten voor opslag die overeenkomen met de momentopnamen die zijn gemaakt tijdens deze periode.
* Incrementele momentopnamen worden opgeslagen als pagina-blobs. Alle gebruikers met behulp van niet-beheerde schijven worden in rekening gebracht voor de momentopnamen die zijn opgeslagen in hun lokale storage-account. Omdat de restore-punt verzamelingen die worden gebruikt door beheerde VM-back-ups blob-momentopnamen op het opslagniveau van de onderliggende, voor beheerde schijven ziet u kosten die overeenkomt met blob-momentopname prijzen en ze zijn.
* Voor premium storage-accounts de momentopnamen die voor instant recovery points aantal voor de limiet van 10 TB van toegewezen ruimte.
* U krijgt een mogelijkheid voor het configureren van de momentopname bewaarperiode op basis van de behoeften van de herstelbewerking. Afhankelijk van het vereiste, kunt u de bewaarperiode momentopname instellen op minimaal één dag in de blade back-upbeleid zoals hieronder wordt uitgelegd. Hiermee kunt u kosten voor het bewaren van momentopname opslaan als u niet vaak herstelbewerkingen uitvoert.


>[!NOTE]
>Met deze direct een upgrade kan de bewaartermijn van de momentopname van alle klanten te herstellen (**nieuwe en bestaande beide worden opgenomen**) wordt ingesteld op een standaardwaarde van twee dagen. U kunt echter de duur instellen aan de hand van uw behoefte aan een waarde tussen 1 tot 5 dagen.


## <a name="cost-impact"></a>Kosten impact

De incrementele momentopnamen worden opgeslagen in de storage-account van de virtuele machine, die worden gebruikt voor direct herstellen. Incrementele momentopnamen betekent dat de ruimte die wordt ingenomen door een momentopname is gelijk aan de ruimte die wordt ingenomen door pagina's die zijn geschreven nadat de momentopname is gemaakt. De kosten zijn nog steeds voor de per GB gebruikte ruimte die wordt ingenomen door de momentopname en de prijs per GB is dezelfde zoals vermeld in de [pagina met prijzen](https://azure.microsoft.com/pricing/details/managed-disks/).


## <a name="upgrading-to-instant-restore"></a>Een upgrade naar Instant Restore

Als u de Azure-portal gebruikt, ziet u een melding op het kluisdashboard. Deze melding is gekoppeld aan ondersteuning voor grote schijven en verbeteringen in back-up en herstel snelheid.
Als u wilt openen op een scherm voor het upgraden van direct herstellen, selecteert u de banner.

![Back-uptaak in VM-back-upstack Resource Manager-implementatiemodel--ondersteuning-melding](./media/backup-azure-vms/instant-rp-banner.png)

Klik op **Upgrade** zoals wordt weergegeven in de onderstaande schermafbeelding:

![Back-uptaak in VM-back-upstack Resource Manager-implementatiemodel--upgrade](./media/backup-azure-vms/instant-rp.png)

U kunt ook, gaat u naar **eigenschappen** pagina van de kluis om op te halen de **Upgrade** onder de optie **VM-back-upstack**.

![Back-uptaak in VM-back-upstack--pagina eigenschappen](./media/backup-azure-vms/instant-restore-capability-properties.png)


## <a name="configure-snapshot-retention-using-azure-portal"></a>Momentopname bewaren met behulp van Azure portal configureren
Deze optie is momenteel beschikbaar in West-Centraal VS, India-Zuid en Australië-Oost.

Voor de bijgewerkte gebruikers in de Azure-portal ziet u een veld toegevoegd aan de **VM back-upbeleid** blade onder de **direct herstellen** sectie. Kunt u de bewaartermijn van de momentopname van de **VM back-upbeleid** blade voor alle virtuele machines die zijn gekoppeld aan de specifieke back-upbeleid.

![Instant Restore-mogelijkheid](./media/backup-azure-vms/instant-restore-capability.png)

## <a name="upgrade-to-instant-restore-using-powershell"></a>Een upgrade uitvoert naar direct herstellen met behulp van PowerShell

Als u wilt de self-service voor stroomactiviteitvoortgang en een upgrade naar direct herstellen, voert u de volgende cmdlets uit vanaf een hogergelegen PowerShell terminal:

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

## <a name="upgrade-to-instant-restore-using-cli"></a>Een upgrade uitvoert naar direct herstellen met behulp van CLI

Voer de volgende opdrachten uit vanuit een shell:

1.  Meld u aan bij uw Azure-account:

    ```
    az login
    ```

2.  Selecteer het abonnement dat u wilt registreren:

    ```
    az account set --subscription "Subscription Name"
    ```

3.  Dit abonnement registreren:

    ```
    az feature register --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
    ```

## <a name="verify-that-the-upgrade-is-successful"></a>Controleer of de upgrade is voltooid

### <a name="powershell"></a>PowerShell
Voer de volgende cmdlet vanaf een verhoogde PowerShell terminal:

```
Get-AzureRmProviderFeature -FeatureName "InstantBackupandRecovery" -ProviderNamespace Microsoft.RecoveryServices
```

### <a name="cli"></a>CLI
Voer de volgende opdracht uit vanuit een shell:

```
az feature show --namespace Microsoft.RecoveryServices --name InstantBackupandRecovery
```

De status 'Registered', wordt vervolgens uw abonnement bijgewerkt naar direct herstellen.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

### <a name="what-are-the-cost-implications-of-instant-restore"></a>Wat zijn de gevolgen van de kosten van Instant restore?
Momentopnamen worden opgeslagen samen met de schijven te maken van het herstelpunt versneld en het terugzetten. Als gevolg hiervan, ziet u kosten voor opslag die met de momentopname bewaarperiode geselecteerd als onderdeel van de virtuele machine back-upbeleid overeenkomen.

### <a name="in-premium-storage-accounts-do-the-snapshots-taken-for-instant-recovery-point-occupy-the-10-tb-snapshot-limit"></a>In Premium Storage-accounts, de momentopnamen voor directe herstelpunt dat de momentopnamelimiet van 10 TB innemen?
Ja, nemen de momentopnamen voor directe herstelpunt voor premium storage-accounts 10 TB van de momentopname van de toegewezen ruimte.

### <a name="how-does-the-snapshot-retention-work-during-the-five-day-period"></a>Hoe werkt de bewaarperiode momentopname tijdens de periode van vijf dagen?
Elke dag een nieuwe momentopname wordt gemaakt, worden er vijf afzonderlijke incrementele momentopnamen. De grootte van de momentopname is afhankelijk van het gegevensverloop die in de meeste gevallen ongeveer 2-7%.

### <a name="is-an-instant-restore-snapshot-an-incremental-snapshot-or-full-snapshot"></a>Een momentopname van een instant restore is een momentopname van incrementele of een volledige momentopname?
Momentopnamen die zijn gemaakt als onderdeel van de directe herstelfuncties zijn incrementele momentopnamen.

### <a name="how-can-i-calculate-the-approximate-cost-increase-due-to-instant-restore-feature"></a>Hoe kan ik de toename van de geschatte kosten vanwege de functie voor direct terugzetten berekenen?
Dat hangt ervan af op het verloop van de virtuele machine. In geen stabiele status hebben, kunt u ervan uitgaan dat de toename van kosten is = momentopname retentie periode dagelijkse verloop per VM-opslagkosten per GB.

### <a name="if-the-recovery-type-for-a-restore-point-is-snapshot-and-vault-and-i-perform-a-restore-operation-which-recovery-type-will-be-used"></a>Als het hersteltype voor een herstelpunt is 'Momentopname en kluis' en ik een herstelbewerking uitvoeren, kunt u welk hersteltype wordt gebruikt?
Als de recovery-type is 'momentopname en kluis', wordt de terugzetten automatisch worden uitgevoerd vanuit de lokale momentopname, die veel sneller wordt vergeleken met de herstelbewerking uitgevoerd vanaf de kluis.

### <a name="what-happens-if-i-select-retention-period-of-restore-point-tier-2-less-than-the-snapshot-tier1-retention-period"></a>Wat gebeurt er als ik de bewaarperiode van herstelpunt (laag 2) kleiner is dan de bewaarperiode van de momentopname (Tier1) Selecteer?
Het nieuwe model is niet toegestaan voor het verwijderen van het herstelpunt (Tier2), tenzij de momentopname (Tier1) is verwijderd. Het is raadzaam om de planning restore point (Tier2)-bewaarperiode groter is dan de bewaarperiode van de momentopname.

### <a name="why-is-my-snapshot-existing-even-after-the-set-retention-period-in-backup-policy"></a>Waarom wordt mijn momentopname bestaande ook na de ingestelde bewaarperiode periode in back-upbeleid?
Als het herstelpunt van de momentopname heeft en dat de meest recente RP beschikbaar is, wordt wel worden bewaard totdat de die er een volgende geslaagde back-up is. Dit is aan de hand van de ontworpen GC beleid vandaag dat ten minste één van de meest recente RP worden altijd aanwezig in het geval alle back-ups meer op vanwege een probleem in de virtuele machine mislukken stuurt. In normale scenario's worden RPs opgeschoond in maximaal 24 uur na de vervaldatum.
