---
title: Een back-upkluis upgraden naar een Recovery Services-kluis | Microsoft Docs
description: Instructies en informatie over ondersteuning voor een upgrade van uw Azure Backup-kluis naar een Recovery Services-kluis.
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
ms.assetid: 228fef19-2f6b-4067-acc3-fb6e501afb88
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 1/4/2018
ms.author: sogup;markgal;arunak
ms.openlocfilehash: 8396a7276fde10eb95a22ed07fa61625acfdd77f
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="upgrade-a-backup-vault-to-a-recovery-services-vault"></a>Een back-upkluis upgraden naar een Recovery Services-kluis

In dit artikel wordt uitgelegd hoe u een back-upkluis bijwerken naar een Recovery Services-kluis. Het upgradeproces heeft geen invloed op een back-uptaken en back-gegevens niet verloren. De belangrijkste redenen voor een upgrade van een back-upkluis naar een Recovery Services-kluis:
- Alle functies van een back-upkluis worden bewaard in een Recovery Services-kluis.
- Recovery Services-kluizen bieden meer functionaliteit dan Backup-kluizen, met inbegrip van: betere beveiliging, geïntegreerd bewaking, snellere herstelacties en herstelacties op itemniveau.
- Back-items beheren vanuit de portal voor een betere en vereenvoudigd.
- Nieuwe functies zijn alleen van toepassing op de Recovery Services-kluizen.

## <a name="impact-to-operations-during-upgrade"></a>Gevolgen voor de bewerkingen tijdens de upgrade

Bij het bijwerken van een back-upkluis naar een Recovery Services-kluis, zijn er geen gevolgen voor uw gegevens vlak-bewerkingen. Alle back-uptaken normaal gaan en eventuele actieve hersteltaken zonder onderbreking voortgezet. Tijdens de upgrade een korte uitvaltijd rekening worden gebracht beheerbewerkingen en u kunt geen nieuwe items beveiligen of ad-hoc taken van de back-ups maken. Het herstellen van taken voor IaaS VM's uitvoeren tijdens de upgrade niet. De upgrade kluis duurt minder dan een uur te voltooien. Eenmaal is voltooid, vervangt een Recovery Services-kluis de Backup-kluis.

## <a name="changes-to-your-automation-and-tool-after-upgrading"></a>Wijzigingen in uw automation en het hulpprogramma na de upgrade

Tijdens het voorbereiden van uw infrastructuur voor de upgrade van de kluis, moet u uw bestaande automatisering of tooling om ervoor te zorgen dat het blijft werken na de upgrade bijwerken.
Raadpleeg de verwijzingen van de PowerShell-cmdlets voor de [Resource Manager-implementatiemodel](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Voordat u een upgrade

Controleer de volgende problemen voordat u uw Backup-kluizen naar de Recovery Services-kluizen bijwerkt.

- **Minimale agentversie**: als u uw kluis upgraden, zorg ervoor dat de Microsoft Azure Recovery Services (MARS)-agent is ten minste versie 2.0.9083.0. Als de MARS-agent ouder dan 2.0.9083.0 is, moet u de agent bijwerken voordat u het upgradeproces start.
- **Op basis van het exemplaar factureringsmodel**: Recovery Services-kluizen ondersteunen alleen het facturering model op basis van het exemplaar. Als er een back-upkluis die door de oudere factureringsmodel gebaseerde opslag wordt gebruikt, moet u het facturering model converteren tijdens de upgrade.
- **Er zijn geen bewerkingen van dagelijkse back-upconfiguratie**: tijdens de upgrade, toegang tot het management vlak wordt beperkt. Voltooi alle beheeracties vlak en start de upgrade.

## <a name="using-powershell-scripts-to-upgrade-your-vaults"></a>Upgrade van uw kluizen met behulp van PowerShell-scripts

U kunt de PowerShell-scripts gebruiken voor een upgrade van de Backup-kluizen naar Recovery Services-kluizen. Controleer of de vereiste onderdelen van PowerShell voor het activeren van de upgrade van de kluis. PowerShell-scripts voor de Backup-kluizen werken niet voor de Recovery Services-kluizen. Bereid uw omgeving de kluizen bijwerken:

1. Installeren of upgraden [Windows Management Framework (WMF) naar versie 5](https://www.microsoft.com/download/details.aspx?id=50395) of hoger.
2. [Installeer Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. Download de [PowerShell-script](https://aka.ms/vaultupgradescript2) uw kluizen bijwerken.

### <a name="run-the-powershell-script"></a>Het PowerShell-script uitvoeren

Het volgende script gebruiken om bij te werken uw kluizen. Het volgende voorbeeldscript heeft uitleg van de parameters.

RecoveryServicesVaultUpgrade 1.0.2.ps1 **- SubscriptionID** `<subscriptionID>` **- VaultName** `<vaultname>` **-locatie** `<location>` **- ResourceType** `BackupVault` **- TargetResourceGroupName**`<rgname>`

**SubscriptionID** -de abonnement-id van de kluis die wordt bijgewerkt.<br/>
**VaultName** -de naam van de Backup-kluis dat wordt bijgewerkt.<br/>
**Locatie** -locatie van de kluis wordt bijgewerkt.<br/>
**ResourceType** -BackupVault gebruiken.<br/>
**TargetResourceGroupName** - omdat u een upgrade de kluis aan een implementatie op basis van een Resource Manager opgeven van een resourcegroep. U kunt een bestaande resourcegroep gebruiken of een door een nieuwe naam maken. Als u de naam van een resourcegroep gespeld, kunt u een nieuwe resourcegroep maken. Lees voor meer informatie over resourcegroepen, dit [overzicht over resourcegroepen](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Namen van de resourcegroep hebben beperkingen. Volg de richtlijnen; niet doet, kan ervoor zorgen dat upgrades van de kluis mislukken.
>
>**Azure US Government** klanten nodig hebben om in te stellen van de omgeving voor 'AzureUSGovernment' tijdens het uitvoeren van het script.
>**Azure China** klanten nodig hebben om in te stellen van de omgeving voor 'AzureChinaCloud' tijdens het uitvoeren van het script.

Het volgende codefragment is een voorbeeld van welke uw PowerShell-opdracht als eruitzien moet:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

U kunt ook het script zonder parameters voor uitvoeren en u wordt gevraagd naar informatie invoeren voor alle vereiste parameters.

Het PowerShell-script vraagt u uw referenties in te voeren. Voer uw referenties tweemaal: eenmaal voor de Service Manager-account en een tweede keer voor de Resource Manager-account.

### <a name="pre-requisites-checking"></a>Vereisten controleren
Nadat u uw Azure-referenties hebt ingevoerd, Azure wordt gecontroleerd of uw omgeving voldoet aan de volgende vereisten:

- **Minimale agentversie** -Backup-kluizen upgraden naar Recovery Services-kluizen moet de MARS-agent ten minste versie 2.0.9083.0. Als u items naar een Backup-kluis geregistreerd met een agent ouder is dan 2.0.9083.0 hebt, wordt de controle van vereisten mislukt. Als de controle op vereisten is mislukt, werk de agent en probeer het opnieuw bijwerken van de kluis. U kunt de nieuwste versie van de agent uit downloaden [http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe](http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **Continu configuratie taken**: als iemand anders is het configureren van taak voor een back-upkluis ingesteld moet worden bijgewerkt of u registreert een item, de controle op vereisten is mislukt. Voltooi de configuratie of klaar bent met het registreren van het item en start het upgradeproces van de kluis.
- **Opslag op basis van een factureringsmodel**: Recovery Services-kluizen ondersteunen het facturering model op basis van het exemplaar. Als u de upgrade van de kluis op een back-upkluis die gebruikmaakt van de opslag op basis van een factureringsmodel uitvoert, wordt u gevraagd om bij te werken uw factureringsmodel samen met de kluis. Anders kunt u het factureringsmodel eerst bijwerken en voer vervolgens de upgrade van de kluis.
- Identificeer een resourcegroep voor de Recovery Services-kluis. Als u wilt profiteren van de implementatiefuncties van Resource Manager, moet u een Recovery Services-kluis in een resourcegroep te plaatsen. Als u niet welke resourcegroep weet wilt gebruiken, Geef een naam en het upgradeproces de resourcegroep voor u gemaakt. Het upgradeproces wordt ook de kluis koppelt aan de nieuwe resourcegroep.

Zodra het upgradeproces is voltooid aan de vereisten te controleren, het proces wordt u gevraagd te starten van de upgrade van de kluis. Nadat u hebt bevestigd, vindt het upgradeproces meestal ongeveer 15-20 minuten duren, afhankelijk van de grootte van uw kluis. Als u een grote kluis hebt, kan upgrade tot 90 minuten duren.

## <a name="managing-your-recovery-services-vaults"></a>Het beheren van de Recovery Services-kluizen

De volgende schermen ziet een nieuwe Recovery Services-kluis bijgewerkt van de Backup-kluis, in de Azure portal. Het eerste scherm toont het kluisdashboard die belangrijke entiteiten voor de kluis wordt weergegeven.

![Voorbeeld van een upgrade uitgevoerd van een back-upkluis Recovery Services-kluis](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

Het tweede scherm ziet u de Help-informatie beschikbaar om u te helpen koppelingen aan de slag met de Recovery Services-kluis.

![Help-koppelingen in de blade snel starten](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Stappen na de upgrade
Recovery Services-kluis ondersteunt geven informatie over de tijdzone in de back-upbeleid. Nadat de kluis upgrade is voltooid, gaat u naar de back-upbeleid in kluis-instellingen en bijwerken van de tijdzone-informatie voor elk van de beleidsregels die zijn geconfigureerd in de kluis. Dit scherm bevat al de back-upschema tijd opgegeven als per lokale tijdzone die wordt gebruikt wanneer u beleid hebt gemaakt. 

## <a name="enhanced-security"></a>Verbeterde beveiliging

Wanneer een back-upkluis is bijgewerkt naar een Recovery Services-kluis, worden de beveiligingsinstellingen voor de kluis die automatisch ingeschakeld. Wanneer de beveiligingsinstellingen op bepaalde bewerkingen zoals het verwijderen van de back-ups, of het wijzigen van een wachtwoordzin vereist een [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) PINCODE. Zie het artikel voor meer informatie over de verbeterde beveiliging [beveiligingsfuncties die beschermen hybride back-ups](backup-azure-security-feature.md). 

Als de verbeterde beveiliging is ingeschakeld, wordt gegevens behouden gedurende 14 dagen na de punt herstelgegevens is verwijderd uit de kluis. Klanten wordt gefactureerd voor opslag van deze beveiligingsgegevens. Beveiliging Gegevensretentie geldt voor de herstelpunten die voor de Azure backup-agent, de Azure Backup-Server en de System Center Data Protection Manager worden uitgevoerd. 

## <a name="gather-data-on-your-vault"></a>Gegevens verzamelen over uw kluis

Wanneer u een naar een Recovery Services-kluis upgrade, rapporten configureren voor Azure Backup (voor IaaS VM's en Microsoft Azure Recovery Services (MARS)) en Power BI gebruiken voor toegang tot de rapporten. Zie het artikel voor meer informatie over het verzamelen van gegevens [Azure Backup configureren rapporten](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**De upgrade-abonnement invloed heeft op Mijn lopende back-ups?**</br>
Nee. Uw actieve back-ups niet wordt onderbroken tijdens en na de upgrade.

**Als ik niet van plan bent over het upgraden van snel, wat gebeurt er met mijn kluizen?**</br>
Omdat alle nieuwe functies alleen voor Recovery Services-kluizen gelden, raden we u aan uw kluizen upgraden. Vanaf 1 September 2017 begint Microsoft back-upkluizen automatisch upgraden naar een Recovery Services-kluizen. Na November 30,2017, kunt u niet langer Backup-kluizen met behulp van PowerShell maken. Uw kluis kan automatisch worden bijgewerkt elk gewenst moment ertussen. Microsoft raadt dat u uw kluis zo snel mogelijk bijwerken.

**Wat houdt de upgrade voor mijn bestaande tooling?**</br>
Werk uw tooling naar het Resource Manager-implementatiemodel. Recovery Services-kluizen zijn gemaakt voor gebruik in het Resource Manager-implementatiemodel. Planning voor het Resource Manager-implementatiemodel en accounting voor het verschil in uw kluizen zijn belangrijk. 

**Tijdens de upgrade is er veel uitvaltijd?**</br>
Dit is afhankelijk van het aantal resources die wordt bijgewerkt. Voor kleinere implementaties (enkele tientallen van beveiligde instanties), moet de hele upgrade minder dan 20 minuten duren. Voor grotere implementaties moet het maximaal een uur duren.

**Kan ik terugdraaien na de upgrade?**</br>
Nee. Terugdraaien wordt niet ondersteund nadat de resources hebt geüpgraded.

**Kan ik mijn abonnement of bronnen om te zien als ze geschikt voor upgrade zijn valideren?**</br>
Ja. De eerste stap bij de upgrade wordt gevalideerd of de bronnen kunnen van upgrade. Als de validatie van vereisten mislukt, kunt u berichten ontvangen voor de redenen die de upgrade kan niet worden voltooid.

**Kan ik mijn back-up op basis van een CSP-kluis bijwerken?**</br>
Nee. U kunt back-upkluizen op basis van een CSP op dit moment niet upgraden. Er wordt ondersteuning toevoegen voor op basis van een CSP Backup-kluizen in de volgende versies upgraden.

**Kan ik mijn klassieke kluis na het bijwerken weergeven?**</br>
Nee. U kunt weergeven of beheren van uw klassieke kluis na het bijwerken. Alleen mogelijk met de nieuwe Azure portal voor alle acties op de kluis.

**De upgrade is mislukt, maar de computer die de agent vereisen vastgehouden niet meer bijgewerkt, bestaat. Wat moet ik doen in dat geval**</br>
Als u moet de store te gebruiken, kunnen de back-ups van deze computer voor lange bewaartermijn, dan hebt u zich niet bijwerken van de kluis. In toekomstige releases voegt ondersteuning voor het upgraden van een dergelijke kluis toe.
Als u niet nodig hebt voor het opslaan van de back-ups van deze machine meer, vervolgens Schakel Hef de registratie van deze machine uit de kluis en probeer de upgrade.

**Waarom zie ik niet de informatie van de taken voor mijn resources na de upgrade?**</br>
Bewaking voor back-ups (MARS-agent en IaaS) is een nieuwe functie die u krijgt wanneer u uw Backup-kluis naar de Recovery Services-kluis bijwerkt. De controlegegevens duurt maximaal 12 uur te synchroniseren met de service.

**Hoe kan ik een probleem melden?**</br>
Als een gedeelte van de kluis upgrade mislukt, ziet u dat de OperationId weergegeven in de fout. Microsoft Support werkt proactief als het probleem wilt oplossen. U kunt ondersteuning bereiken of een e-mail sturen naar rsvaultupgrade@service.microsoft.com met uw abonnements-ID, de kluisnaam van de en OperationId. Er wordt geprobeerd het probleem zo snel mogelijk oplossen. Probeer de bewerking niet tenzij expliciet om dit te doen door Microsoft.


## <a name="next-steps"></a>Volgende stappen
Gebruik het volgende artikel op:</br>
[Back-up van een IaaS VM](backup-azure-arm-vms-prepare.md)</br>
[Back-up van een Azure Backup-Server](backup-azure-microsoft-azure-backup.md)</br>
[Maak een back-up van een Windows-Server](backup-configure-vault.md).
