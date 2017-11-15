---
title: Beheren en controleren van de virtuele machine van Azure back-ups | Microsoft Docs
description: Meer informatie over het beheren en controleren van een back-ups van virtuele machine van Azure
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e3d3de79c7f2465791ec68f850df2fc6317880f9
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-the-classic-portal"></a>Algemene taken voor Azure Backup en trigger waarschuwingen in de klassieke portal beheren
> [!div class="op_single_selector"]
> * [Back-ups van virtuele machine in Azure beheren](backup-azure-manage-vms.md)
> * [Klassieke VM-back-ups beheren](backup-azure-manage-vms-classic.md)
>
>

In dit artikel bevat informatie over algemene beheer en controle taken voor het klassieke model virtuele machines in Azure beveiligde.  

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Zie [voorbereiden van uw back-up van virtuele machines in Azure-omgeving](backup-azure-vms-prepare.md) voor meer informatie over het werken met Classic deployment model virtuele machines.
>
> [!IMPORTANT]
>Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen.
>
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Na 30 November 2017 zich u niet PowerShell gebruiken voor het maken van de Backup-kluizen. **Per 30 November 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.

## <a name="manage-protected-virtual-machines"></a>Beveiligde virtuele machines beheren
Voor het beheren van beveiligde virtuele machines:

1. Weergeven en beheren van instellingen voor een virtuele machine back-up klikt u op de **beveiligde Items** tabblad.
2. Klik op de naam van een beveiligde item om te zien de **back-Details** tabblad u informatie over de laatste back-up ziet.

    ![Back-up van virtuele machine](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. Weergeven en beheren van back-upbeleid voor een virtuele machine klikt u op de **beleid** tabblad.

    ![Beleid voor de virtuele machine](./media/backup-azure-manage-vms/manage-policy-settings.png)

    De **back-upbeleid** tabblad ziet u het bestaande beleid. U kunt zo nodig wijzigen. Als u wilt maken van een nieuw beleid Klik **maken** op de **beleid** pagina. Houd er rekening mee dat als u wilt verwijderen van een beleid voor deze mag niet gekoppeld aan virtuele machines.

    ![Beleid voor de virtuele machine](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. U kunt meer informatie over acties of status ophalen voor een virtuele machine op de **taken** pagina. Klik op een taak in de lijst voor meer informatie of filter de taken voor een specifieke virtuele machine.

    ![Taken](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Op aanvraag back-up van een virtuele machine
U kunt op aanvraag back-up van een virtuele machine uitvoeren zodra deze is geconfigureerd voor beveiliging. Als de eerste back-up in behandeling is voor de virtuele machine, maakt back-up op aanvraag een volledige kopie van de virtuele machine in Azure Backup-kluis. Als de eerste back-up is voltooid, is op aanvraag back-up worden alleen wijzigingen voor het verzenden van eerdere back-up voor Azure backup-kluis dat wil zeggen het altijd incrementeel.

> [!NOTE]
> De bewaartermijn van een op aanvraag back-up is ingesteld op bewaren waarde die is opgegeven voor het dagelijkse bewaren in back-upbeleid overeenkomt met de virtuele machine.  
>
>

Te ondernemen op aanvraag back-up van een virtuele machine:

1. Navigeer naar de **beveiligde Items** pagina en selecteer **Azure virtuele Machine** als **Type** (indien nog niet geselecteerd) en klik op **Selecteer** de knop.

    ![VM-Type](./media/backup-azure-manage-vms/vm-type.png)
2. Selecteer de virtuele machine waarop u wilt uitvoeren op aanvraag back-up en klik op **nu back-** knop aan de onderkant van de pagina.

    ![Nu een back-maken](./media/backup-azure-manage-vms/backup-now.png)

    Hiermee wordt een back-uptaak gemaakt op de geselecteerde virtuele machine. De bewaartermijn van het herstelpunt dat is gemaakt door middel van deze taak wordt niet hetzelfde zijn dat is opgegeven in het beleid dat is gekoppeld aan de virtuele machine.

    ![Maken van back-uptaak](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > Als u wilt weergeven van het beleid is gekoppeld aan een virtuele machine, Inzoomen op virtuele machine in de **beveiligde Items** pagina en Ga naar de back-upbeleid tabblad.
   >
   >
3. Zodra de taak is gemaakt, kunt u klikken op **taak weergeven** knop in de pop-up klikken om te zien van de bijbehorende taak op de pagina taken.

    ![back-uptaak gemaakt](./media/backup-azure-manage-vms/created-job.png)
4. Nadat de taak is voltooid, wordt u een herstelpunt gemaakt waarin u kunt de virtuele machine te herstellen. Dit wordt ook waarde herstelpunt kolom verhoogd met 1 in **beveiligde Items** pagina.

## <a name="stop-protecting-virtual-machines"></a>Stop de beveiliging van virtuele machines
U kunt kiezen om te stoppen van de toekomstige back-ups van een virtuele machine met de volgende opties:

* Bewaren van back-upgegevens die zijn gekoppeld aan virtuele machine in Azure Backup-kluis
* Back-upgegevens die zijn gekoppeld aan virtuele machine verwijderen

Als u hebt geselecteerd voor het bewaren van back-upgegevens die zijn gekoppeld aan virtuele machine, kunt u de back-upgegevens te herstellen van de virtuele machine. Voor prijsinformatie voor zulke virtuele machines, klikt u op [hier](https://azure.microsoft.com/pricing/details/backup/).

Beveiliging voor een virtuele machine stoppen:

1. Navigeer naar **beveiligde Items** pagina en selecteer **virtuele machine van Azure** als de filtertype (indien nog niet geselecteerd) en klik op **Selecteer** knop.

    ![VM-Type](./media/backup-azure-manage-vms/vm-type.png)
2. Selecteer de virtuele machine en klik op **beveiliging stoppen** aan de onderkant van de pagina.

    ![Stop de beveiliging](./media/backup-azure-manage-vms/stop-protection.png)
3. Standaard Azure Backup niet de back-gegevens verwijderen die zijn gekoppeld aan de virtuele machine.

    ![Stop de beveiliging bevestigen](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Als u back-upgegevens verwijderen wilt, selecteert u het selectievakje in.

    ![Selectievakje](./media/backup-azure-manage-vms/checkbox.png)

    Selecteer een reden voor het stoppen van de back-up. Dit is optioneel, helpt bieden een reden Azure Backup om te werken op de feedback en prioriteren van de klant scenario's.
4. Klik op **indienen** knop verzenden de **beveiliging stoppen** taak. Klik op **taak weergeven** om te zien van de bijbehorende de taak in **taken** pagina.

    ![Stop de beveiliging](./media/backup-azure-manage-vms/stop-protect-success.png)

    Als u niet hebt geselecteerd **verwijderen die zijn gekoppeld back-upgegevens** optie tijdens **beveiliging stoppen** wizard, dan zal na een opdracht is voltooid, protection status is gewijzigd in **beveiliging gestopt**. Tot deze expliciet worden verwijderd, blijven de gegevens met Azure Backup. U kunt de gegevens altijd verwijderen door het selecteren van de virtuele machine in de **beveiligde Items** pagina en te klikken op **verwijderen**.

    ![Gestopte beveiliging](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Als u hebt geselecteerd de **verwijderen die zijn gekoppeld back-upgegevens** optie, de virtuele machine niet deel uit van de **beveiligde Items** pagina.

## <a name="re-protect-virtual-machine"></a>Virtuele machine opnieuw te beveiligen
Als u niet hebt geselecteerd de **verwijderen koppelen back-upgegevens** optie in **beveiliging stoppen**, kunt u de virtuele machine opnieuw beveiligen door de stappen die vergelijkbaar is met een back-up geregistreerde virtuele machines. Als beveiligd, deze virtuele machine back-upgegevens bewaard voordat stop de beveiliging hebben en herstelpunten zijn gemaakt na het opnieuw te beveiligen.

Na het opnieuw te beveiligen, de beveiligingsstatus van de virtuele machine wordt gewijzigd in **beveiligde** als er herstelpunten voorafgaand aan **beveiliging stoppen**.

  ![Opnieuw beveiligde virtuele machine](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Wanneer de virtuele machine opnieuw te beveiligen, kunt u een ander beleid dan het beleid waarmee virtuele machine in eerste instantie is beveiligd.
>
>

## <a name="unregister-virtual-machines"></a>Hef de registratie van virtuele machines
Als u verwijderen van de virtuele machine uit de back-upkluis wilt:

1. Klik op de **UNREGISTER** knop aan de onderkant van de pagina.

    ![Schakel de beveiliging](./media/backup-azure-manage-vms/unregister-button.png)

    Een toast-melding wordt weergegeven onder aan het scherm bevestiging aanvraagt. Klik op **Ja** om door te gaan.

    ![Schakel de beveiliging](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Back-up van gegevens verwijderen
U kunt de back-upgegevens die zijn gekoppeld aan een virtuele machine, ofwel verwijderen:

* Tijdens de taak beveiliging stoppen
* Na een stop de beveiliging is taak voltooid op een virtuele machine

Verwijderen van de back-upgegevens op een virtuele machine bevindt zich in de *beveiliging gestopt* status na de voltooiing van een **back-up stoppen** taak:

1. Navigeer naar de **beveiligde Items** pagina en selecteer **Azure virtuele Machine** als *type* en klik op de **Selecteer** knop.

    ![VM-Type](./media/backup-azure-manage-vms/vm-type.png)
2. Selecteer de virtuele machine. De virtuele machine zich in **beveiliging gestopt** status.

    ![Beveiliging is gestopt](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Klik op de **verwijderen** knop aan de onderkant van de pagina.

    ![Back-up verwijderen](./media/backup-azure-manage-vms/delete-backup.png)
4. In de **verwijderen van de back-upgegevens** wizard, selecteert u een reden voor het verwijderen van de back-upgegevens (sterk aanbevolen) en klik op **indienen**.

    ![Back-upgegevens verwijderen](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Hiermee wordt een taak voor het verwijderen van de back-upgegevens van de geselecteerde virtuele machine gemaakt. Klik op **taak weergeven** om te zien van de bijbehorende taak in de pagina taken.

    ![Verwijderen van gegevens is voltooid](./media/backup-azure-manage-vms/delete-data-success.png)

    Zodra de taak is voltooid, kunt u de vermelding die overeenkomt met de virtuele machine wordt verwijderd uit **beveiligde items** pagina.

## <a name="dashboard"></a>Dashboard
Op de **Dashboard** pagina die u informatie over virtuele machines in Azure, de opslag- en taken die zijn gekoppeld aan deze in de afgelopen 24 uur controleren kunt. U kunt back-status en eventuele bijbehorende back-fouten weergeven.

![Dashboard](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Waarden in het dashboard worden elke 24 uur vernieuwd.
>
>

## <a name="auditing-operations"></a>Controle-bewerkingen
Azure backup biedt een overzicht van de 'bewerking logs' van de back-upbewerkingen geactiveerd door de klant zodat u eenvoudig zien precies welke bewerkingen zijn uitgevoerd op de back-upkluis. Operations logs geweldige postmortemkeuring inschakelen en ondersteuning voor de back-upbewerkingen controleren.

De volgende bewerkingen worden vastgelegd in Logboeken:

* Registreren
* Registratie ongedaan maken
* Beveiliging configureren
* Back-up (zowel gepland en op aanvraag back-ups via BackupNow)
* Herstellen
* Stop de beveiliging
* Back-upgegevens verwijderen
* Beleid toevoegen
* Beleid verwijderen
* Bijwerken van beleid
* Taak annuleren

Bewerkingslogboeken overeenkomt met een back-upkluis weergeven:

1. Navigeer naar **beheerservices** in Azure-portal en klik op de **Bewerkingslogboeken** tabblad.

    ![Bewerkingslogboeken](./media/backup-azure-manage-vms/ops-logs.png)
2. Selecteer in de filters **back-** als *Type* en geef de naam van de back-upkluis in *servicenaam* en klik op **indienen**.

    ![Bewerking Logboeken filteren](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. Selecteer elke bewerking in de operations-logboeken en klik op **Details** om details overeenkomt met een bewerking te bekijken.

    ![Details van bewerking logboeken ophalen](./media/backup-azure-manage-vms/ops-logs-details.png)

    De **Details wizard** bevat informatie over de werking is geactiveerd, taak-Id, de resource waarop deze bewerking wordt geactiveerd en de begintijd van de bewerking.

    ![Details van bewerking](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Meldingen van waarschuwingen
U kunt aangepaste meldingen van waarschuwingen voor de taken krijgen in de portal. Dit wordt bereikt door het definiëren van regels voor waarschuwingen op basis van PowerShell van operationele Logboeken gebeurtenissen. Wordt u aangeraden *PowerShell versie 1.3.0 of hoger*.

Een voorbeeld van een opdracht, om te definiëren van een aangepaste melding om te waarschuwen voor mislukte back-ups, ziet er als:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: kunt u dit via krijgen Operations Logs pop zoals beschreven in de bovenstaande sectie. ResourceUri die in details pop-upvenster van een bewerking is de ResourceId van de voor deze cmdlet worden opgegeven.

**OperationName**: dit is de indeling ' Microsoft.Backup/backupvault/<EventName>' indien EventName een van de registratie, Unregister, ConfigureProtection, back-up, herstel, StopProtection, DeleteBackupData, CreateProtectionPolicy is, DeleteProtectionPolicy, UpdateProtectionPolicy

**Status**: ondersteunde waarden zijn-gestart, is geslaagd en mislukt.

**ResourceGroup**: ResourceGroup van de resource waarop de bewerking wordt geactiveerd. U kunt dit verkrijgen van ResourceId waarde. De waarde tussen velden */resourceGroups/* en */providers/* in ResourceId is de waarde voor ResourceGroup.

**Naam**: naam van de waarschuwingsregel.

**CustomEmail**: Geef de aangepaste e-mailadres waarnaar u wilt verzenden waarschuwingsmeldingen

**SendToServiceOwners**: deze optie wordt een waarschuwing melding verzonden voor alle beheerders en medebeheerders van het abonnement. Kan worden gebruikt in **nieuw AzureRmAlertRuleEmail** cmdlet

### <a name="limitations-on-alerts"></a>Beperkingen voor waarschuwingen
Waarschuwingen op basis van gebeurtenissen zijn onderworpen aan de volgende beperkingen:

1. Waarschuwingen worden verzonden op alle virtuele machines in de back-upkluis. U kunt deze voor waarschuwingen voor een specifieke set van virtuele machines in een back-upkluis niet aanpassen.
2. Deze functie is in Preview. [Meer informatie](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. U ontvangt waarschuwingen van de 'alerts-noreply@mail.windowsazure.com'. U kunt de afzender voor e-mailadres momenteel niet wijzigen.

## <a name="next-steps"></a>Volgende stappen
* [Herstellen van virtuele machines in Azure](backup-azure-restore-vms.md)
