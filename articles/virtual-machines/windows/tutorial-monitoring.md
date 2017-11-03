---
title: Azure controleren en bijwerken en virtuele Machines van Windows | Microsoft Docs
description: Zelfstudie - controleren en bijwerken van een virtuele Windows-Machine met Azure PowerShell
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: a37aed8b3321d3518ffd73e09f5bb21266a7e577
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>Controleren en bijwerken van een virtuele Windows-Machine met Azure PowerShell

Agents Azure-bewaking worden gebruikt voor opstart-en prestatiegegevens verzamelen van Azure Virtual machines, deze gegevens opslaan in Azure-opslag en zorg toegankelijk zijn via de portal, Azure PowerShell-module en de Azure CLI. Updatebeheer kunt u updates en patches voor uw Windows Azure-VM's beheren.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op een virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens weergeven VM-host
> * Installeren van de extensie voor diagnostische gegevens
> * Metrische gegevens weergeven VM
> * Maken van een waarschuwing
> * Windows-updates beheren
> * Geavanceerde controle instellen

Voor deze zelfstudie is moduleversie 3,6 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Als u het voorbeeld in deze zelfstudie, moet u een bestaande virtuele machine hebben. Indien nodig, dit [voorbeeldscript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kunt maken voor u. Als u werkt de zelfstudie, vervangt u de resourcegroep, de naam van de virtuele machine en de locatie waar nodig.

## <a name="view-boot-diagnostics"></a>Diagnostische gegevens over opstarten bekijken

Als Windows virtuele machines opstarten, worden de diagnostische agent opstarten schermuitvoer die kan worden gebruikt voor het oplossen van doel. Deze mogelijkheid is standaard ingeschakeld. De vastgelegde schermopnamen worden opgeslagen in een Azure storage-account wordt ook standaard gemaakt. 

U krijgt de diagnostische gegevens van opstarten met het [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) opdracht. In het volgende voorbeeld wordt de diagnostische gegevens over opstarten worden gedownload naar de hoofdmap van de * c:\* station. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Metrische gegevens weergeven host

Een virtuele machine van Windows heeft een specifieke Host-virtuele machine in Azure die deze met samenwerkt. Metrische gegevens worden automatisch verzameld voor de Host, en kunnen worden weergegeven in de Azure-portal.

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op **metrische gegevens** op de blade virtuele machine en selecteer vervolgens een van de Host metrische gegevens onder **beschikbare metrische gegevens** om te zien hoe de VM-Host wordt uitgevoerd.

    ![Metrische gegevens weergeven host](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Installeren van de extensie voor diagnostische gegevens

De basic host metrische gegevens zijn beschikbaar, maar meer gedetailleerd en VM-specifieke metrische gegevens en u moet de extensie Azure diagnostics installeren op de virtuele machine. De extensie Azure diagnostics kan aanvullende controle en diagnostische gegevens worden opgehaald van de virtuele machine. U kunt deze maatstaven voor prestaties weergeven en waarschuwingen op basis van de manier waarop de virtuele machine uitvoert. De diagnostische extensie wordt geïnstalleerd via de Azure portal als volgt:

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op **diagnose instellingen**. De lijst ziet u dat *opstarten diagnostics* al uit de vorige sectie zijn ingeschakeld. Klik op het selectievakje voor *basismetrieken*.
3. Klik op de **gastniveau bewaking inschakelen** knop.

    ![Diagnostische metrische gegevens weergeven](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Metrische gegevens weergeven VM

U kunt de metrische gegevens van virtuele machine op dezelfde manier als u de host VM metrische gegevens hebt bekeken bekijken:

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens **myVM** in de lijst met resources.
2. Als u wilt zien hoe de virtuele machine wordt uitgevoerd, klikt u op **metrische gegevens** op de blade virtuele machine en selecteer vervolgens een van de diagnostische gegevens metrische gegevens onder **beschikbare metrische gegevens**.

    ![Metrische gegevens weergeven VM](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Waarschuwingen maken

U kunt waarschuwingen op basis van specifieke prestatiewaarden kunt maken. Waarschuwingen kunnen worden gebruikt om te waarschuwen wanneer de gemiddelde CPU-gebruik overschrijdt een bepaalde drempelwaarde of beschikbare vrije schijfruimte zakt onder een bepaalde hoeveelheid. Waarschuwingen worden weergegeven in de Azure-portal of kunnen worden verzonden via e-mail. U kunt ook Azure Automation-runbooks of Azure Logic Apps activeren in reactie op waarschuwingen worden gegenereerd.

Het volgende voorbeeld wordt een waarschuwing voor het gemiddelde CPU-gebruik.

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op **waarschuwing regels** op de VM-blade en klik vervolgens op **metrische waarschuwing toevoegen** aan de bovenkant van de waarschuwingen-blade.
4. Geef een **naam** voor de waarschuwing, zoals *myAlertRule*
5. Als u wilt een waarschuwing activeert wanneer CPU-percentage 1.0 gedurende vijf minuten overschrijdt, laat u alle overige standaardwaarden geselecteerd.
6. Indien gewenst, schakel het selectievakje voor *e-eigenaren, bijdragers en lezers* e-mailmelding verzenden. De standaardactie is dat een melding in de portal.
7. Klik op de knop **OK**.

## <a name="manage-windows-updates"></a>Windows-updates beheren

Updatebeheer kunt u updates en patches voor uw Windows Azure-VM's beheren.
Rechtstreeks van uw virtuele machine, u kunt snel de status van de beschikbare updates te beoordelen, installatie van vereiste updates plannen en Implementatieresultaten om te controleren of updates met succes zijn toegepast op de virtuele machine bekijken.

Zie voor informatie over prijzen, [Automation prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Updatebeheer inschakelen

Schakel beheer van de Update voor uw virtuele machine:
 
1. Aan de linkerkant van het scherm en selecteer **virtuele machines**.
2. Selecteer een virtuele machine in de lijst.
3. Op het scherm VM in de **Operations** sectie, klikt u op **updatebeheer**. De **updatebeheer inschakelen** scherm wordt geopend.

Validatie is uitgevoerd om te bepalen of het beheer van updates is ingeschakeld voor deze virtuele machine. De validatie worden controles uitgevoerd voor een werkruimte voor logboekanalyse en gekoppelde Automation-account, en als de oplossing in de werkruimte is.

Een werkruimte voor logboekanalyse wordt voor het verzamelen van gegevens die worden gegenereerd door de functies en services zoals updatebeheer gebruikt. De werkruimte biedt één locatie om te bekijken en analyseren van gegevens uit meerdere bronnen. Aanvullende als actie wilt uitvoeren op virtuele machines die updates vereisen, kunt Azure Automation u het uitvoeren van scripts uitvoeren op virtuele machines, zoals downloaden en toepassen van updates.

Tijdens het validatieproces controleert ook als de virtuele machine is ingericht met de Microsoft Monitoring Agent (MMA) en hybride worker. Deze agent wordt gebruikt om te communiceren met de virtuele machine en informatie opvragen over de status van de update. 

Als niet aan deze vereisten wordt voldaan, verschijnt er een banner waarin u de optie voor het inschakelen van de oplossing.

![Update Management vrijgeven configuratie banner](./media/tutorial-monitoring/manageupdates-onboard-solution-banner.png)

Klik op de banner om de oplossing in te schakelen. Als een van de volgende vereisten zijn gevonden te zijn verdwenen na de validatie, worden ze automatisch toegevoegd:

* [Meld u Analytics](../../log-analytics/log-analytics-overview.md) werkruimte
* [Automatisering](../../automation/automation-offering-get-started.md)
* Een [hybride runbook worker](../../automation/automation-hybrid-runbook-worker.md) is ingeschakeld op de virtuele machine

De **updatebeheer inschakelen** scherm wordt geopend. Configureer de instellingen en klik op **inschakelen**.

![Voor updatebeheer inschakelen](./media/tutorial-monitoring/manageupdates-update-enable.png)

Inschakelen van de oplossing, kan maximaal 15 minuten duren en gedurende deze tijd moet u het browservenster niet sluiten. Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates op de virtuele machine met logboekanalyse loopt.
Het kost tussen 30 minuten en 6 uur voor de gegevens beschikbaar zijn voor analyse.

### <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

 ![Status van de update weergeven](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie na uw release-planning en servicevenster.
U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Bijvoorbeeld, u kunt opnemen kritieke of updatepakketten voor beveiligingsupdates en uitsluiten.

Een nieuwe Update-implementatie plannen voor de virtuele machine door te klikken op **schema-update-implementatie** boven aan de **updatebeheer** scherm. In de **New-update-implementatie** scherm, geeft u de volgende informatie:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Updateclassificatie** -Selecteer de typen van de update-implementatie die is opgenomen in de implementatie van software. De classificatietypen zijn:
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
U ziet dat de **geplande** tabel ziet u het schema voor u gemaakt.

> [!WARNING]
> Voor updates waarvoor opnieuw opstarten, de virtuele machine wordt automatisch opnieuw gestart.

### <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout is opgetreden met één of meer updates in de implementatie, wordt de status **Gedeeltelijk mislukt**.
Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

   ![Update-implementatie status dashboard voor specifieke implementatie](./media/tutorial-monitoring/manageupdates-view-results.png)

In **Updateresultaten** tegel is een overzicht van het totale aantal updates en van de Implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* **Niet geprobeerd** -de update is niet geïnstalleerd omdat er onvoldoende tijd is beschikbaar op basis van de duur van het venster Onderhoud gedefinieerd.
* **Geslaagd** -de update is voltooid
* **Kan geen** -de update is mislukt

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de **uitvoer** tegel voor taakstroom van het runbook verantwoordelijk voor het beheren van de implementatie van de update op de doel-virtuele machine.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="advanced-monitoring"></a>Geavanceerde controle 

U kunt doen meer geavanceerde bewaking van uw virtuele machine met behulp van [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Als u dit nog niet hebt gedaan, kunt u zich aanmelden voor een [gratis proefversie](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) van Operations Management Suite.

Wanneer u toegang tot de OMS-portal hebt, kunt u de werkruimte-id en werkruimtesleutel kunt vinden op de blade instellingen. Gebruik de [Set AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) opdracht de OMS-extensie toevoegen aan de virtuele machine. Bijwerken van de waarden van variabelen in het onderstaande voorbeeld om weer te geven u werkruimtesleutel OMS en werkruimte-id.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Na een paar minuten ziet u de nieuwe virtuele machine in de OMS-werkruimte. 

![OMS-blade](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt geconfigureerd en virtuele machines in Azure Security Center gecontroleerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtueel netwerk maken
> * Een resourcegroep en de virtuele machine maken 
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens weergeven host
> * Installeren van de extensie voor diagnostische gegevens
> * Metrische gegevens weergeven VM
> * Maken van een waarschuwing
> * Windows-updates beheren
> * Geavanceerde controle instellen

Ga naar de volgende zelfstudie voor meer informatie over Azure security center.

> [!div class="nextstepaction"]
> [VM-beveiliging beheren](./tutorial-azure-security.md)