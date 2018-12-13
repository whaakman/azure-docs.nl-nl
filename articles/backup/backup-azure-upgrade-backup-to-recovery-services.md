---
title: Een back-upkluis upgraden naar een Recovery Services-kluis
description: Instructies en informatie over ondersteuning aan uw Azure Backup-kluis upgraden naar een Recovery Services-kluis.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 1/4/2018
ms.author: sogup
ms.openlocfilehash: 41a826304af338814666e80dfaf584021809dbb0
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52880043"
---
# <a name="upgrade-a-backup-vault-to-a-recovery-services-vault"></a>Een back-upkluis upgraden naar een Recovery Services-kluis

In dit artikel wordt uitgelegd hoe u een back-upkluis upgraden naar een Recovery Services-kluis. Het upgradeproces dit geen invloed op alle lopende back-uptaken en back-gegevens niet verloren. De belangrijkste redenen voor een back-upkluis upgraden naar een Recovery Services-kluis:
- Alle functies van een back-upkluis worden bewaard in een Recovery Services-kluis.
- Recovery Services-kluizen bieden meer functionaliteit dan back-upkluizen, met inbegrip van: betere beveiliging, geïntegreerde bewaking, snellere herstelacties en herstelacties op itemniveau.
- Back-upitems van een verbeterde, vereenvoudigde portal beheren.
- Nieuwe functies zijn alleen van toepassing op de Recovery Services-kluizen.

## <a name="impact-to-operations-during-upgrade"></a>Gevolgen voor de bewerkingen tijdens de upgrade

Wanneer een back-upkluis upgraden naar een Recovery Services-kluis, zijn er geen gevolgen voor uw gegevens vlak-bewerkingen. Alle back-uptaken normaal gaan en eventuele actieve hersteltaken blijven zonder onderbreking. Tijdens de upgrade, management-bewerkingen in rekening worden gebracht een korte uitvaltijd, en u kunt geen nieuwe items beveiligen of ad-hoc taken van de back-ups maken. Het herstellen van taken voor IaaS-VM's uitvoeren tijdens de upgrade niet. De upgrade van de kluis duurt minder dan een uur om te voltooien. Wanneer u klaar bent, is een Recovery Services-kluis wordt vervangen door de Backup-kluis.

## <a name="changes-to-your-automation-and-tool-after-upgrading"></a>Wijzigingen in uw automatisering en hulpprogramma na de upgrade

Tijdens het voorbereiden van uw infrastructuur voor de upgrade van de kluis, moet u uw bestaande automatisering of hulpprogramma's om ervoor te zorgen dat het blijft werken na de upgrade bijwerken.
Raadpleeg de verwijzingen van de PowerShell-cmdlets voor de [Resource Manager-implementatiemodel](backup-client-automation.md).


## <a name="before-you-upgrade"></a>Voordat u een upgrade uitvoert

Controleer de volgende problemen voordat u uw back-upkluizen naar Recovery Services-kluizen upgraden.

- **Minimale agentversie**: als u uw kluis bijwerken, zorg ervoor dat de Microsoft Azure Recovery Services agent (MARS) is ten minste versie 2.0.9083.0. Als de MARS-agent ouder dan 2.0.9083.0 is, moet u de agent bijwerken voordat u begint met het upgradeproces.
- **Exemplaar op basis van facturering van maandabonnementen**: Recovery Services-kluizen ondersteunen alleen het exemplaar op basis van factureringsmodel. Als u een back-upkluis die van het oude facturering op basis van een Storage-model gebruikmaakt hebt, converteert u de facturering van maandabonnementen tijdens de upgrade.
- **Er zijn geen bewerkingen van dagelijkse back-upconfiguratie**: tijdens de upgrade, toegang tot de beheerlaag wordt beperkt. Voltooi alle beheeracties vlak en start de upgrade.

## <a name="using-powershell-scripts-to-upgrade-your-vaults"></a>Uw upkluizen upgraden met behulp van PowerShell-scripts

PowerShell-scripts kunt u uw back-upkluizen upgraden naar Recovery Services-kluizen. Controleer of de vereiste onderdelen voor PowerShell voor het activeren van de upgrade van de kluis. PowerShell-scripts voor back-upkluizen werken niet voor de Recovery Services-kluizen. Uw omgeving voorbereiden op de upkluizen upgraden:

1. Installeren of upgraden [Windows Management Framework (WMF) versie 5](https://www.microsoft.com/download/details.aspx?id=50395) of hoger.
2. [Installeer Azure PowerShell-MSI](https://github.com/Azure/azure-powershell/releases/download/v3.8.0-April2017/azure-powershell.3.8.0.msi).
3. Download de [PowerShell-script](https://aka.ms/vaultupgradescript2) om bij te werken uw kluizen.

### <a name="run-the-powershell-script"></a>Het PowerShell-script uitvoeren

Het volgende script gebruiken om te upgraden van uw kluizen. Het volgende voorbeeldscript biedt uitleg van de parameters.

RecoveryServicesVaultUpgrade 1.0.2.ps1 **- SubscriptionID** `<subscriptionID>` **- VaultName** `<vaultname>` **-locatie** `<location>` **- ResourceType** `BackupVault` **- TargetResourceGroupName** `<rgname>`

**Abonnements-id** -de abonnement-ID-nummer van de kluis die wordt bijgewerkt.<br/>
**VaultName** -de naam van de Backup-kluis die wordt bijgewerkt.<br/>
**Locatie** -locatie van de kluis wordt bijgewerkt.<br/>
**ResourceType** -BackupVault gebruiken.<br/>
**TargetResourceGroupName** , omdat u een upgrade de kluis aan een Resource Manager gebaseerde implementatie uitvoert, Geef een resourcegroep. U kunt een bestaande resourcegroep gebruiken, of maakt u er een door een nieuwe naam te geven. Als u de naam van een resourcegroep een spelfout, kunt u een nieuwe resourcegroep maken. Lees voor meer informatie over resourcegroepen, dit [overzicht van de resourcegroepen](../azure-resource-manager/resource-group-overview.md#resource-groups).

>[!NOTE]
> Namen van de resourcegroep hebben beperkingen. Volg de richtlijnen; dit niet doet, kan ertoe leiden dat upgrades van de kluis mislukken.
>
>**Azure US Government** moeten klanten de omgeving 'AzureUSGovernment' instellen tijdens het uitvoeren van het script.
>**Azure China** moeten klanten de omgeving 'AzureChinaCloud' instellen tijdens het uitvoeren van het script.

Het volgende codefragment wordt een voorbeeld van wat uw PowerShell-opdracht moet er als volgt uitzien:

```
RecoveryServicesVaultUpgrade.ps1 -SubscriptionID 53a3c692-5283-4f0a-baf6-49412f5ebefe -VaultName "TestVault" -Location "Australia East" -ResourceType BackupVault -TargetResourceGroupName "ContosoRG"
```

U kunt ook het script zonder parameters uitvoert en u wordt gevraagd naar informatie invoeren voor alle vereiste parameters.

Het PowerShell-script vraagt u uw referenties in te voeren. Voer uw referenties tweemaal: eenmaal voor de Service Manager-account en een tweede keer voor de Resource Manager-account.

### <a name="pre-requisites-checking"></a>Vereisten controleren
Nadat u uw Azure-referenties hebt ingevoerd, controleert Azure of uw omgeving voldoet aan de volgende vereisten:

- **Minimale agentversie** -back-upkluizen upgraden naar Recovery Services-kluizen vereist dat de MARS-agent moet ten minste versie 2.0.9083.0. Als u items naar een Backup-kluis geregistreerd met een agent ouder is dan 2.0.9083.0 hebt, wordt de controle mislukt. Als de controle mislukt, de agent wordt bijgewerkt en probeer het opnieuw bijwerken van de kluis. U kunt de meest recente versie van de agent uit downloaden [ http://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe ](https://download.microsoft.com/download/F/4/B/F4B06356-150F-4DB0-8AD8-95B4DB4BBF7C/MARSAgentInstaller.exe).
- **Continue configuratie taken**: als iemand configureert u de taak voor een back-upkluis instellen om te worden bijgewerkt of registreren van een item, de controle is mislukt. Voltooi de configuratie of klaar bent met het registreren van het item en start vervolgens het upgradeproces van de kluis.
- **Opslag op basis van facturering van maandabonnementen**: Recovery Services-kluizen ondersteunen het exemplaar op basis van factureringsmodel. Als u de upgrade van de kluis op een back-upkluis die gebruikmaakt van de opslag op basis van facturering van maandabonnementen uitvoert, wordt u gevraagd om bij te werken uw factureringsmodel samen met de kluis. Anders kunt u uw factureringsmodel eerst, bijwerken en voer vervolgens de upgrade van de kluis.
- Identificeer een resourcegroep voor de Recovery Services-kluis. Om te profiteren van de implementatiefuncties van de Resource Manager-, moet u een Recovery Services-kluis in een resourcegroep geplaatst. Als u niet welke resourcegroep u wilt gebruiken weet, Geef een naam en het upgradeproces de resourcegroep voor u gemaakt. Het upgradeproces worden ook de kluis koppelt aan de nieuwe resourcegroep.

Als tijdens de upgrade is voltooid voor het controleren van de vereisten, het proces wordt u gevraagd te starten van de upgrade van de kluis. Nadat u hebt bevestigd, duurt het upgradeproces doorgaans ongeveer 15-20 minuten om te voltooien, afhankelijk van de grootte van uw kluis. Als u een grote kluis hebt, kan upgrade tot 90 minuten duren.

## <a name="managing-your-recovery-services-vaults"></a>Beheren van uw Recovery Services-kluizen

De volgende schermen weer een nieuwe Recovery Services-kluis, een upgrade uitgevoerd van de Backup-kluis in Azure portal. Het eerste scherm ziet u het dashboard van de kluis waarin belangrijke entiteiten voor de kluis worden weergegeven.

![Voorbeeld van een upgrade uitgevoerd van een Backup-kluis van Recovery Services-kluis](./media/backup-azure-upgrade-backup-to-recovery-services/upgraded-rs-vault-in-dashboard.png)

De tweede schermafbeelding geeft de help van koppelingen om u te helpen aan de slag met behulp van de Recovery Services-kluis.

![Help-koppelingen in de blade snel starten](./media/backup-azure-upgrade-backup-to-recovery-services/quick-start-w-help-links.png)

## <a name="post-upgrade-steps"></a>Stappen na de upgrade
Recovery Services-kluis biedt ondersteuning voor informatie over de tijdzone van het opgeven in de back-upbeleid. Nadat de kluis upgrade is voltooid, gaat u naar de back-upbeleid in kluismenu-instellingen en bijwerken van de informatie over de tijdzone voor elk van de beleidsregels die zijn geconfigureerd in de kluis. Dit scherm bevat al de back-upschema-tijd opgegeven als per lokale tijdzone die wordt gebruikt wanneer u beleid hebt gemaakt. 

## <a name="enhanced-security"></a>Verbeterde beveiliging

Wanneer een back-upkluis is bijgewerkt naar een Recovery Services-kluis, worden de beveiligingsinstellingen voor deze kluis automatisch ingeschakeld. Als de beveiligingsinstellingen zijn op bepaalde bewerkingen zoals het verwijderen van back-ups of het wijzigen van een wachtwoordzin moet een [Azure multi-factor Authentication](../active-directory/authentication/multi-factor-authentication.md) PINCODE. Zie het artikel voor meer informatie over de verbeterde beveiliging [beveiligingsfuncties die beschermen hybride back-ups](backup-azure-security-feature.md). 

Wanneer de verbeterde beveiliging is ingeschakeld, worden gegevens van bewaard gedurende 14 dagen na de punt herstelgegevens is verwijderd uit de kluis. Klanten worden in rekening gebracht voor opslag van deze beveiligingsgegevens. Beveiliging het bewaren van gegevens is van toepassing op de herstelpunten die voor de Azure backup-agent, de Azure Backup Server en de System Center Data Protection Manager worden uitgevoerd. 

## <a name="gather-data-on-your-vault"></a>Verzamelen van gegevens voor uw kluis

Nadat u een naar een Recovery Services-kluis upgrade, rapporten configureren voor Azure Backup (voor IaaS-VM's en Microsoft Azure Recovery Services (MARS)) en Power BI gebruiken voor toegang tot de rapporten. Zie het artikel voor meer informatie over het verzamelen van gegevens, [configureren van Azure Backup-rapporten](backup-azure-configure-reports.md).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**De upgrade-abonnement invloed heeft op mijn back-ups?**</br>
Nee. Uw back-ups worden onderbroken tijdens en na de upgrade.

**Als ik niet van plan bent voor het upgraden van snel, wat gebeurt er met mijn kluizen?**</br>
Omdat alle nieuwe functies alleen voor de Recovery Services-kluizen gelden, raden we u aan uw upkluizen upgraden. Vanaf 1 September 2017, zal Microsoft beginnen met back-upkluizen automatisch upgraden naar Recovery Services-kluizen. Na November 30,2017, u kunt niet meer met behulp van PowerShell back-up-kluizen maken. Uw kluis kan worden automatisch bijgewerkt telkens wanneer in tussen. Microsoft raadt dat u uw kluis zo snel mogelijk upgraden.

**Wat doet deze upgrade gemiddelde voor mijn bestaande tooling?**</br>
Werk uw hulpprogramma's voor op het Resource Manager-implementatiemodel. Recovery Services-kluizen zijn gemaakt voor gebruik in het Resource Manager-implementatiemodel. Planning voor het Resource Manager-implementatiemodel en accounting voor het verschil in uw kluizen is belangrijk. 

**Tijdens de upgrade is er veel downtime?**</br>
Dat hangt ervan af op het aantal resources dat wordt bijgewerkt. Voor kleinere implementaties (enkele tientallen beveiligde exemplaren), moet de volledige upgrade minder dan 20 minuten duren. Voor grotere implementaties moet het maximaal een uur duren.

**Kan ik terugdraaien na de upgrade?**</br>
Nee. Terugdraaien wordt niet ondersteund wanneer de resources zijn bijgewerkt.

**Kan ik mijn abonnement of resources om te zien of ze geschikt van upgrade valideren?**</br>
Ja. De eerste stap bij een upgrade van valideert dat de resources geschikt voor upgrade zijn. Als de validatie van vereisten mislukt, ontvangt u berichten over alle redenen waarom die de upgrade kan niet worden voltooid.

**Kan ik mijn back-up op basis van een CSP-kluis upgraden?**</br>
Nee. U kunt back-upkluizen op basis van een CSP op dit moment niet bijwerken. Ondersteuning voor het op basis van een CSP Backup-kluizen in de volgende versies upgraden, zullen we toevoegen.

**Kan ik mijn klassieke kluis na het bijwerken bekijken?**</br>
Nee. U kunt geen weergeven of beheren van uw klassieke kluis na het bijwerken. Alleen mogelijk met de nieuwe Azure-portal voor alle acties op de kluis.

**De upgrade is mislukt, maar de computer die de agent die niet meer bijgewerkt, bestaat. Wat moet ik doen in dat geval?**</br>
Als u gebruiken wilt, kunnen de back-ups van deze machine voor langetermijnretentie, dan hebt u zich niet bijwerken van de kluis. In toekomstige versies zullen we ondersteuning voor het upgraden van een dergelijke kluis toevoegen.
Als u niet nodig hebt voor het opslaan van de back-ups van deze machine niet meer, vervolgens registratie van deze machine uit de kluis en voer de upgrade uit.

**Waarom zie ik niet de informatie van de taken voor mijn resources na de upgrade?**</br>
Controle van het back-ups (MARS-agent en IaaS) is een nieuwe functie die u ontvangt wanneer u uw back-upkluis naar Recovery Services-kluis upgraden. De informatie voor prestatiebewaking duurt tot 12 uur te synchroniseren met de service.

**Hoe meld ik een probleem?**</br>
Als een gedeelte van de upgrade van de kluis is mislukt, houd er rekening mee dat de bewerkings-ID in de volgende fout weergegeven. Microsoft Support werkt proactief als het probleem wilt oplossen. U kunt contact opnemen met ondersteuning of een e-mail verzenden naar rsvaultupgrade@service.microsoft.com met uw abonnements-ID, de naam van de kluis en de bewerkings-id. We zullen proberen om het probleem zo snel mogelijk opgelost. Probeer de bewerking niet tenzij expliciet om dit te doen door Microsoft.


## <a name="next-steps"></a>Volgende stappen
Gebruik het volgende artikel:</br>
[Back-up van een IaaS-VM](backup-azure-arm-vms-prepare.md)</br>
[Back-up van een Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Maak een back-up van een Windows-Server](backup-configure-vault.md).
