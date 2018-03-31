---
title: Azure controleren en bijwerken en virtuele Machines van Windows | Microsoft Docs
description: Zelfstudie - controleren en bijwerken van een virtuele Windows-Machine met Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b908e8877162a6a1d9292616a1704c1c528e1725
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>Controleren en bijwerken van een virtuele Windows-Machine met Azure PowerShell

Agents Azure-bewaking worden gebruikt voor opstart-en prestatiegegevens verzamelen van Azure Virtual machines, deze gegevens opslaan in Azure-opslag en zorg toegankelijk zijn via de portal, Azure PowerShell-module en de Azure CLI. Updatebeheer kunt u updates en patches voor uw Windows Azure-VM's beheren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op een virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens weergeven VM-host
> * Installeren van de extensie voor diagnostische gegevens
> * Metrische gegevens over de VM weergeven
> * Maken van een waarschuwing
> * Windows-updates beheren
> * Wijzigingen van de monitor en inventarisatie
> * Geavanceerde bewaking instellen

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine hebben. Indien nodig kan dit [voorbeeldscript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) er een voor u maken. Als u werkt de zelfstudie, vervangt u de resourcegroep, de naam van de virtuele machine en de locatie waar nodig.

## <a name="view-boot-diagnostics"></a>Diagnostische gegevens over opstarten bekijken

Als Windows virtuele machines opstarten, worden de diagnostische agent opstarten schermuitvoer die kan worden gebruikt voor het oplossen van doel. Deze mogelijkheid is standaard ingeschakeld. De vastgelegde schermopnamen worden opgeslagen in een Azure storage-account wordt ook standaard gemaakt.

U krijgt de diagnostische gegevens van opstarten met het [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) opdracht. In het volgende voorbeeld wordt de diagnostische gegevens over opstarten worden gedownload naar de hoofdmap van de * c:\* station.

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Metrische gegevens over de host weergeven

Een virtuele machine van Windows heeft een specifieke Host-virtuele machine in Azure die deze met samenwerkt. Metrische gegevens worden automatisch verzameld voor de Host, en kunnen worden weergegeven in de Azure-portal.

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op **metrische gegevens** op de blade virtuele machine en selecteer vervolgens een van de Host metrische gegevens onder **beschikbare metrische gegevens** om te zien hoe de VM-Host wordt uitgevoerd.

    ![Metrische gegevens over de host weergeven](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>De extensie voor diagnostische gegevens installeren

De basic host metrische gegevens zijn beschikbaar, maar meer gedetailleerd en VM-specifieke metrische gegevens en u moet de extensie Azure diagnostics installeren op de virtuele machine. Met de Azure Diagnostics-extensie kunnen aanvullende bewakings- en diagnostische gegevens worden opgehaald van de virtuele machine. U kunt deze maatstaven voor prestaties weergeven en waarschuwingen maken op basis van de manier waarop de virtuele machine presteert. De Azure Diagnostics-extensie wordt als volgt geïnstalleerd via Azure Portal:

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op **Diagnose-instellingen**. In de lijst ziet u dat *Diagnostische gegevens over opstarten* al is ingeschakeld in de vorige sectie. Klik op het selectievakje voor *Basismetrieken*.
3. Klik op de **gastniveau bewaking inschakelen** knop.

    ![Metrische gegevens over de diagnose weergeven](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Metrische gegevens over de VM weergeven

U kunt de metrische gegevens over de virtuele machine op dezelfde manier weergeven als u de metrische gegevens over de host-VM hebt weergegeven:

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
2. Als u wilt zien hoe de VM wordt uitgevoerd, klikt u op **Metrische gegevens** op de blade van de virtuele machine en selecteert u vervolgens een van de metrische gegevens onder **Beschikbare metrische gegevens**.

    ![Metrische gegevens over de VM weergeven](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Waarschuwingen maken

U kunt waarschuwingen maken op basis van specifieke maatstaven voor prestaties. Waarschuwingen kunnen worden gebruikt om u een melding te sturen wanneer het gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt of de beschikbare vrije schijfruimte onder een bepaalde hoeveelheid komt. Waarschuwingen worden weergegeven in Azure Portal of kunnen worden verzonden via e-mail. U kunt ook Azure Automation-runbooks of Azure Logic Apps activeren als reactie op het genereren van waarschuwingen.

In het volgende voorbeeld wordt een waarschuwing gemaakt voor het gemiddelde CPU-gebruik.

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op de VM-blade op **Waarschuwingsregels** en klik vervolgens boven aan de waarschuwingenblade op **Waarschuwing voor metrische gegevens toevoegen**.
3. Geef een **Naam** op voor de waarschuwing, zoals *myAlertRule*
4. Als u een waarschuwing wilt activeren wanneer het CPU-percentage gedurende vijf minuten 1.0 overschrijdt, laat u alle overige standaardwaarden geselecteerd.
5. Schakel desgewenst het selectievakje voor *E-mailadressen van eigenaren, bijdragers en lezers* in om een e-mailmelding te verzenden. Standaard wordt een melding in de portal weergegeven.
6. Klik op de knop **OK**.

## <a name="manage-windows-updates"></a>Windows-updates beheren

Updatebeheer kunt u updates en patches voor uw Windows Azure-VM's beheren.
U kunt rechtstreeks vanuit uw VM de status van de beschikbare updates beoordelen, de installatie van vereiste updates plannen en de implementatieresultaten bekijken om te controleren of updates correct zijn toegepast op de VM.

Zie [Automation-prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/) voor prijsinformatie.

### <a name="enable-update-management"></a>Updatebeheer inschakelen

Schakel beheer van de Update voor uw virtuele machine:

1. Selecteer aan de linkerkant van het scherm **Virtuele machines**.
2. Selecteer een VM in de lijst.
3. Klik op het VM-scherm in de sectie **Bewerkingen** op **Updatebeheer**. Het scherm **Updatebeheer inschakelen** wordt weergegeven.

Er wordt een validatie uitgevoerd om te bepalen of updatebeheer is ingeschakeld voor deze virtuele machine.
De validatie bevat controles voor een Log Analytics-werkruimte en het gekoppelde Automation-account en controleert of de oplossing zich in de werkruimte bevindt.

Een [Log Analytics](../../log-analytics/log-analytics-overview.md)-werkruimte wordt gebruikt om gegevens te verzamelen die worden gegenereerd door functies en services zoals Updatebeheer.
De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren.
Als u aanvullende bewerkingen wilt uitvoeren op virtuele machines die updates vereisen, biedt Azure Automation de mogelijkheid runbooks uit te voeren met VM's, zoals updates downloaden en toepassen.

Tijdens het validatieproces wordt ook gecontroleerd of de virtuele machine is ingericht met Microsoft Monitoring Agent (MMA) en Automation Hybrid Runbook Worker.
Deze agent wordt gebruikt om te communiceren met de VM en om informatie op te vragen over de status van de update.

Kies de Log Analytics-werkruimte en het Automation-account en klik op **Inschakelen** om de oplossing in te schakelen. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

Als een van de volgende vereiste onderdelen ontbreekt na de onboarding, wordt dit automatisch toegevoegd:

* [Log Analytics](../../log-analytics/log-analytics-overview.md)-werkruimte
* [Automatisering](../../automation/automation-offering-get-started.md)
* Een [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) wordt ingeschakeld op de VM.

Het scherm **Updatebeheer** wordt geopend. Configureer de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt en klik op **Inschakelen**. Als de velden lichtgrijs zijn, betekent dit dat een andere automatiseringsoplossing is ingeschakeld voor de virtuele machine en dat dezelfde werkruimte en hetzelfde Automation-account moeten worden gebruikt.

![Oplossing voor updatebeheer inschakelen](./media/tutorial-monitoring/manageupdates-update-enable.png)

Het inschakelen van de oplossing kan maximaal 15 minuten duren. Gedurende deze tijd mag u het browservenster niet sluiten. Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates op de VM naar Log Analytics verzonden. Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

### <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven. Nadat de evaluatie van updates voltooid is, ziet u een lijst met ontbrekende updates op de **ontbrekende updates** tabblad.

 ![Updatestatus bekijken](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie na uw release-planning en servicevenster. U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Plan een nieuwe update-implementatie voor de VM door te klikken op **Update-implementatie plannen** boven aan het scherm **Updatebeheer**. Geef de volgende gegevens op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Updateclassificatie**: selecteer de typen software die de update-implementatie moet opnemen in de implementatie. De classificatietypen zijn:
  * Essentiële updates
  * Beveiligingsupdates
  * Updatepakketten
  * Functiepakketten
  * Servicepacks
  * Definitie-updates
  * Hulpprogramma's
  * Updates

* **Schema-instellingen**: u kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd) of een andere tijd opgeven.
  U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen. Klik op de optie Terugkerend onder Terugkeerpatroon om een terugkerende planning in te stellen.

  ![Scherm met instellingen voor de updateplanning](./media/tutorial-monitoring/manageupdates-schedule-win.png)

* **Onderhoudsvenster (minuten)**: geef op binnen welke periode de update-implementatie moet plaatsvinden.  Dit zorgt ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde servicevensters.

Nadat u klaar bent met het configureren van de planning, klikt u op de knop **Maken** en gaat u terug naar het statusdashboard.
U ziet dat de tabel **Gepland** de implementatieplanning weergeeft die u hebt gemaakt.

> [!WARNING]
> Voor updates waarvoor opnieuw opstarten, de virtuele machine wordt automatisch opnieuw gestart.

### <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout is opgetreden met één of meer updates in de implementatie, wordt de status **Gedeeltelijk mislukt**.
Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

![Statusdashboard voor update-implementatie voor specifieke implementatie](./media/tutorial-monitoring/manageupdates-view-results.png)

Op de tegel **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van de opgegeven onderhoudsperiode.
* **Geslaagd**: de update is voltooid.
* **Mislukt**: de update is mislukt.

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de tegel **Uitvoer** om de taakstroom te bekijken van het runbook dat verantwoordelijk is voor het beheer van de implementatie van de updates op de doel-VM.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="monitor-changes-and-inventory"></a>Wijzigingen van de monitor en inventarisatie

U kunt inventarisgegevens verzamelen en weergeven voor software, bestanden, Linux-daemons, Windows-services en Windows-registersleutels op uw computers. Door de configuraties van uw computer bij te houden, is het voor u gemakkelijker vast te stellen waar in uw omgeving zich operationele problemen voordoen, en kunt u beter beoordelen wat de status van uw computers is.

### <a name="enable-change-and-inventory-management"></a>Schakel wijzigings-en inventarisatie

Schakel wijzigings- en inventaris management voor uw virtuele machine:

1. Selecteer aan de linkerkant van het scherm **Virtuele machines**.
2. Selecteer een VM in de lijst.
3. Op het scherm VM in de **Operations** sectie, klikt u op **inventaris** of **bijhouden van wijzigingen**. De **wijzigingen bijhouden inschakelen en voorraad** scherm wordt geopend.

Configureer de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt en klik op **Inschakelen**. Als de velden lichtgrijs zijn, betekent dit dat een andere automatiseringsoplossing is ingeschakeld voor de virtuele machine en dat dezelfde werkruimte en hetzelfde Automation-account moeten worden gebruikt. Eventhough de oplossingen zijn afzonderlijke in het menu, dezelfde oplossing. Als een, ingeschakeld voor uw virtuele machine.

![Wijzigings- en Voorraadbeheer inschakelen](./media/tutorial-monitoring/manage-inventory-enable.png)

Nadat de oplossing is ingeschakeld kan het enige tijd duren terwijl inventaris worden verzameld op de virtuele machine voordat gegevens worden weergegeven.

### <a name="track-changes"></a>Wijzigingen bijhouden

Op uw virtuele machine selecteert **bijhouden** onder **OPERATIONS**. Klik op **instellingen bewerken**, wordt de **bijhouden** pagina wordt weergegeven. Selecteer het type van de instelling die u wilt bijhouden en klik vervolgens op **+ toevoegen** om de instellingen te configureren. De beschikbare opties voor Windows zijn:

* Windows Registry
* Windows-bestanden

Voor gedetailleerde informatie over wijzigingen bijhouden Zie [wijzigingen op een virtuele machine oplossen](../../automation/automation-tutorial-troubleshoot-changes.md)

### <a name="view-inventory"></a>Inventaris weergeven

Op uw virtuele machine selecteert **inventaris** onder **OPERATIONS**. Op het tabblad **Software** wordt een lijst weergegeven met de software die is gevonden. De algemene details voor elke softwarerecord worden in de tabel weergegeven. Deze gegevens omvatten de naam van de software, versie, uitgever en de tijd van laatste vernieuwd.

![Inventaris weergeven](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Monitor activiteitenlogboeken en wijzigingen

Selecteer **Verbinding met het activiteitenlogboek beheren** op de pagina **Wijzigingen bijhouden** op de virtuele machine. Hiermee opent u de pagina **Azure-activiteitenlogboek**. Selecteer **Verbinden** om Wijzigingen bijhouden te verbinden met het Azure-activiteitenlogboek voor uw virtuele machine.

Terwijl deze instelling is ingeschakeld, gaat u naar de pagina **Overzicht** voor de virtuele machine en selecteert u **Stoppen** om de virtuele machine te stoppen. Wanneer u daarom wordt gevraagd, selecteert u **Ja** om de virtuele machine te stoppen. Wanneer deze toewijzing ongedaan is gemaakt, selecteert u **Starten** om de virtuele machine opnieuw op te starten.

Wanneer een virtuele machine wordt gestart en gestopt, wordt een gebeurtenis geregistreerd in het activiteitenlogboek. Ga terug naar de pagina **Wijzigingen bijhouden**. Selecteer het tabblad **Gebeurtenissen** onderaan op de pagina. Na een tijdje worden de gebeurtenissen weergegeven in de grafiek en in de tabel. Elke gebeurtenis kan worden geselecteerd om gedetailleerde informatie over de gebeurtenis weer te geven.

![Wijzigingen in het gebeurtenissenlogboek weergeven](./media/tutorial-monitoring/manage-activitylog-view-results.png)

De grafiek toont wijzigingen die in de loop der tijd hebben plaatsgevonden. Nadat u een verbinding met een Azure-activiteitenlogboek hebt toegevoegd, toont de lijngrafiek bovenaan gebeurtenissen uit het Azure-actitviteitenlogboek. Elke rij in het staafdiagram vertegenwoordigt een ander type wijziging dat kan worden gevolgd. Deze typen zijn Linux-daemons, bestanden, Windows-registersleutels, software en Windows-services. Het tabblad met wijzigingen bevat details over de wijzigingen in de visualisatie. Deze worden getoond in aflopende volgorde vanaf het moment waarop de wijziging optrad (meest recente eerst).

## <a name="advanced-monitoring"></a>Geavanceerde controle

U kunt doen meer geavanceerde bewaking van uw virtuele machine met behulp van de oplossingen zoals updatebeheer en wijzigings- en geleverd door Azure Automation-inventarisatie. [Operations Management Suite](../../automation/automation-intro.md).

Wanneer u toegang tot de werkruimte voor logboekanalyse hebt, kunt u de werkruimte-id en werkruimtesleutel op vinden selecteren **geavanceerde instellingen** onder **instellingen**. Gebruik de [Set AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) opdracht naar de Microsoft Monitoring agent-extensie aan de virtuele machine toevoegen. Bijwerken van de waarden van variabelen in het onderstaande voorbeeld om weer te geven u werkruimtesleutel Log Analytics en werkruimte-id.

```powershell
$workspaceId = "<Replace with your workspace Id>"
$key = "<Replace with your primary key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $workspaceId} `
  -ProtectedSettings @{"workspaceKey" = $key} `
  -Location eastus
```

Na een paar minuten ziet u de nieuwe virtuele machine in de werkruimte logboek Anaytics.

![OMS-blade](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt geconfigureerd en virtuele machines in Azure Security Center gecontroleerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtueel netwerk maken
> * Een resourcegroep en de virtuele machine maken
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens over de host weergeven
> * Installeren van de extensie voor diagnostische gegevens
> * Metrische gegevens over de VM weergeven
> * Maken van een waarschuwing
> * Windows-updates beheren
> * Wijzigingen van de monitor en inventarisatie
> * Geavanceerde bewaking instellen

Ga naar de volgende zelfstudie voor meer informatie over Azure security center.

> [!div class="nextstepaction"]
> [VM-beveiliging beheren](./tutorial-azure-security.md)