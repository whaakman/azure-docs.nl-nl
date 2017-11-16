---
title: Een Site Recovery-kluis upgraden naar een Azure Recovery Services-kluis
description: Meer informatie over het bijwerken van een Azure Site Recovery-kluis naar een Recovery Services-kluis
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/15/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: aa4b82fdf50eca382c4c84610a5f4857106c6d6d
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a>Een Site Recovery-kluis upgraden naar een Azure Resource Manager gebaseerde Recovery Services-kluis

In dit artikel wordt beschreven hoe Azure Site Recovery kluizen bijwerken naar Azure Resource Manager gebaseerde Recovery Services-kluizen zonder invloed op de lopende replicatie. Zie voor meer informatie over Azure Resource Manager-functies en voordelen [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Inleiding
Een Recovery Services-kluis is een Azure Resource Manager-bron voor het beheren van back-up en herstel na noodgevallen systeemeigen in de cloud. Het is een uniform kluis die u in de nieuwe Azure portal gebruiken kunt en vervangt de klassieke back-up en Site Recovery-kluizen.

Recovery Services-kluizen bieden een matrix van functies, waaronder:

* Ondersteuning van Azure Resource Manager: U kunt beveiligen en uw virtuele machines en fysieke machines failover in een Azure Resource Manager-stack.

* Schijf uitsluiten: als er tijdelijke bestanden of hoge verloop gegevens die u niet wilt dat uw bandbreedte voor gebruiken, kunt u de volumes uitsluiten van replicatie. Deze mogelijkheid is ingeschakeld op dit moment in *VMware naar Azure* en *Hyper-V naar Azure* en wordt uitgebreid naar andere scenario's ook.

* Ondersteuning voor premium en lokaal redundante opslag: U kunt nu servers beveiligen in premium-opslag accounts waarmee klanten te beschermen toepassingen met hogere i/o-bewerkingen per seconde (IOPS). Deze mogelijkheid is momenteel ingeschakeld in *VMware naar Azure*.

* Gestroomlijnde ervaring aan de slag: de uitgebreide ervaring van aan de slag is ontworpen voor herstel na noodgevallen setup te vereenvoudigen.

* Back-up en het beheer van de Site Recovery uit de kluis van dezelfde: U kunt nu servers voor herstel na noodgevallen beveiligen of back-up van dezelfde kluis, waardoor uw beheer overhead aanzienlijk kan verminderen.

Zie voor meer informatie over de bijgewerkte ervaring en -functies, de [opslag, back-up en herstel blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Opvallende kenmerken

* Er zijn geen gevolgen voor de lopende replicatie: lopende replicaties doorgaan zonder een onderbreking tijdens en na de upgrade.

* Kan zonder extra kosten: ophalen van een volledige set van bijgewerkte mogelijkheden zonder extra kosten.

* Er is geen gegevensverlies: omdat dit proces een upgrade en niet een migratie is, bestaande herstelpunten voor replicatie en -instellingen blijven behouden tijdens en na de upgrade.


## <a name="what-happens-during-the-vault-upgrade"></a>Wat gebeurt er tijdens de upgrade van de kluis?

Tijdens de upgrade kunt u bewerkingen zoals het registreren van een nieuwe server of het inschakelen van replicatie voor een virtuele machine (VM) niet uitvoeren. Bewerkingen met betrekking tot het lezen van gegevens uit of schrijven van gegevens naar de kluis, zoals continue replicatie van beveiligde items naar de kluis wordt onderbroken.

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a>Wijzigingen in automation en tooling na de upgrade
Wanneer u het type kluis vanuit het klassieke implementatiemodel naar het Resource Manager-implementatiemodel upgraden, de bestaande automatisering of tooling om ervoor te zorgen dat het blijft werken na de upgrade bijwerken.

### <a name="prepare-your-environment-for-the-upgrade"></a>Uw omgeving voorbereiden voor de upgrade

* [Installeer PowerShell of upgrade uitvoeren naar versie 5 of hoger](https://www.microsoft.com/download/details.aspx?id=50395)
* [Installeer de nieuwste versie van Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases)
* [Het script voor het bijwerken van Recovery Services-kluis downloaden](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Vereisten
Als u wilt upgraden van Site Recovery-kluizen naar Azure Resource Manager gebaseerde Recovery Services-kluizen, moet u voldoen aan de volgende vereisten:

* Minimale agent-versie: de versie van Azure Site Recovery Provider is geïnstalleerd op de server moet 5.1.1700.0 of hoger.

* Ondersteunde configuratie: U kunt uw kluis niet configureren met storage area network (SAN) of SQL Server AlwaysOn-beschikbaarheidsgroepen. Alle andere configuraties worden ondersteund.

    >[!NOTE]
    >Na de upgrade kunt u de toewijzing van opslag via de PowerShell beheren.

* Ondersteunde implementatiescenario: uw kluis mag niet de *VMware naar Azure* verouderde implementatiemodel. Voordat u doorgaat, wordt eerst naar de uitgebreide implementatiemodel verplaatsen.

* Geen actieve gebruiker gestart taken die betrekking hebben op management vlak operations: omdat toegang tot het management vlak tijdens de upgrade beperkt is, Voer uw acties voor de vlak management voordat u de upgrade activeren. Dit proces omvat niet lopende replicatie.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Deze upgrade invloed heeft op Mijn lopende replicatie?**

Nee. De lopende replicatie blijft onderbroken tijdens en na de upgrade.

**Wat gebeurt er netwerkinstellingen zoals site-naar-site VPN- en IP-instellingen?**

De upgrade heeft geen invloed op de netwerkinstellingen. Alle Azure-naar-on-premises verbindingen blijven behouden.

**Wat gebeurt er met mijn kluizen als ik niet van plan bent om bij te werken in de nabije toekomst?**

Ondersteuning voor de Site Recovery-kluis in de oude Azure portal zullen worden afgeschaft vanaf September 2017. Het is raadzaam dat u de functie bijwerken naar de nieuwe portal.

**Wat betekent dit migratieplan voor mijn bestaande tooling?**  

Bijwerken van uw tooling naar het Resource Manager-implementatiemodel is een van de belangrijkste wijzigingen die u in uw upgrade plan moet rekening houden. De Recovery Services-kluizen zijn gebaseerd op het Resource Manager-implementatiemodel. 

**Hoe lang duurt de uitvaltijd management vlak laatste?**

De upgrade duurt normaal gesproken ongeveer 15 tot 30 minuten en het maximaal één uur kan duren.

**Kan ik terugdraaien na de upgrade?**

Nee. Terugdraaien wordt niet ondersteund nadat de resources hebt geüpgraded.

**Kan ik mijn abonnement of bronnen om te zien of ze kunnen worden bijgewerkt valideren?**

Ja. In het platform ondersteund upgrade-optie is de eerste stap bij de upgrade te valideren dat de resources geschikt voor een upgrade zijn. Als de validatie mislukt, ontvangt u passende foutberichten of waarschuwingen.

**Hoe meld ik een probleem met de upgrade**

Als er fouten tijdens de upgrade optreden, noteert u de bewerkings-ID die opgenomen in de fout. Microsoft Support werkt proactief over het oplossen van het probleem. U kunt ook contact opnemen met het ondersteuningsteam met uw abonnements-ID, de kluisnaam en de bewerking-ID. Los het probleem zo snel mogelijk werkt ondersteuning. Probeer de bewerking niet tenzij u expliciet moet dit doen door Microsoft.

## <a name="run-the-script"></a>Het script uitvoeren

Voer in PowerShell de volgende opdracht:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* SubscriptionID: De abonnements-ID die is gekoppeld aan de kluis die u een upgrade uitvoert.

* VaultName: De naam van de kluis die u een upgrade uitvoert.

* Locatie: De locatie van de kluis die u een upgrade uitvoert.

* ResourceType: HyperVRecoveryManagerVault voor Site Recovery-kluizen.

* TargetResourceGroupName: De resourcegroep waarin u wilt dat de bijgewerkte kluis worden geplaatst. TargetResourceGroupName kan een bestaande resourcegroep in Azure Resource Manager of een nieuwe zijn. Als de TargetResourceGroupName dat wordt meegeleverd niet bestaat, wordt deze gemaakt als onderdeel van de upgrade op dezelfde locatie als de kluis. Zie voor meer informatie de sectie 'Resourcegroepen' [overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Naamgeving van de resource-groep is onderworpen aan bepaalde beperkingen. Zorg dat u de naamconventie zorgvuldig bekijken om te voorkomen dat de upgrade mislukt kluis.
    >
    >Bijvoorbeeld:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-locatie 'Noord-Europa' - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc

U kunt ook het volgende script uitvoeren. Geef de waarden voor de vereiste parameters.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. Het PowerShell-script vraagt u uw referenties in te voeren. Voer deze twee keer eenmaal voor het klassieke implementatiemodel model-account en eenmaal voor de Azure Resource Manager-account.

2. Nadat u uw referenties hebt ingevoerd, is het script wordt uitgevoerd een selectievakje om te bepalen of de instellingen van uw infrastructuur voldoet aan de eerder vermelde vereisten.

3. Nadat de vereisten zijn gecontroleerd en bevestigd, wordt u gevraagd om door te gaan met de upgrade van de kluis. Het upgradeproces start een upgrade van uw kluis. De volledige upgrade kunt 15 tot 30 minuten duren.

4. Nadat de upgrade is voltooid, kunt u de bijgewerkte kluis in de nieuwe Azure portal openen.

## <a name="post-upgrade-vault-management"></a>Na de upgrade kluis management

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a>Repliceren met behulp van Azure Site Recovery in de Recovery Services-kluis

* U kunt nu uw Azure VM's beveiligen tegen één regio naar een andere. Zie voor meer informatie [Azure-machines repliceren tussen regio's met Azure Site Recovery](site-recovery-azure-to-azure.md).

* Zie voor meer informatie over virtuele VMware-machines repliceren naar Azure, [VMware-machines repliceren naar Azure met Site Recovery](vmware-walkthrough-overview.md).

* Zie voor meer informatie over Hyper-V-machines (zonder VMM) repliceren naar Azure, [repliceren Hyper-V virtuele machines (zonder VMM) naar Azure](hyper-v-site-walkthrough-overview.md).

* Zie voor meer informatie over Hyper-V-machines (met VMM) repliceren naar Azure, [repliceren Hyper-V virtuele machines in VMM-clouds naar Azure met Site Recovery in de Azure portal](vmm-to-azure-walkthrough-overview.md).

* Zie voor meer informatie over Hyper-VM's (met VMM) repliceren naar een secundaire site [repliceren Hyper-V virtuele machines in VMM-clouds naar een secundaire VMM-site met de Azure portal](site-recovery-vmm-to-vmm.md).

* Zie voor meer informatie over virtuele VMware-machines repliceren naar een secundaire site [repliceren lokale virtuele VMware-machines of fysieke servers naar een secundaire site in de klassieke Azure portal](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>De gerepliceerde items weergeven

De volgende afbeelding toont de dashboardpagina van de Recovery Services-kluis die belangrijke entiteiten voor de kluis wordt weergegeven. Selecteer voor een lijst met beveiligde entiteiten in de kluis, **siteherstel** > **gerepliceerde items**.


![Gerepliceerde items](./media/upgrade-site-recovery-vaults/replicateditems.png)

De volgende afbeelding toont de werkstroom voor het weergeven van uw gerepliceerde items en de **Failover** opdracht voor het initiëren van een failover.

![Gerepliceerde items](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Replicatie-instellingen weergeven

In de Site Recovery-kluis is elke beveiligingsgroep worden geconfigureerd met kopieerfrequentie, herstelpunt bewaartermijn, frequentie van toepassingsconsistente momentopnamen en andere replicatie-instellingen. In de Recovery Services-kluis, worden deze instellingen zijn geconfigureerd als een beleid voor wachtwoordreplicatie. De naam van het beleid is de naam van de beveiligingsgroep of de *primarycloud_Policy*.

Zie voor meer informatie over beleid voor wachtwoordreplicatie [replicatiebeleid beheren voor VMware naar Azure](site-recovery-setup-replication-settings-vmware.md).
