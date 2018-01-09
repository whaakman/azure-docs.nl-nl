---
title: Bewaken en bijwerken van Linux virtuele machines in Azure | Microsoft Docs
description: Informatie over het bewaken van diagnostische gegevens over opstarten en maatstaven voor prestaties en pakket-updates op een virtuele Linux-machine in Azure beheren
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: cde484dd59ec6e2821678766726c02362222d496
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Het controleren en bijwerken van een virtuele Linux-machine in Azure

U kunt dat uw virtuele machines (VM's) in Azure correct worden uitgevoerd, zodat boot diagnostics, maatstaven voor prestaties bekijken en beheren van updates van pakket. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens weergeven host
> * Extensie voor diagnostische gegevens op de virtuele machine inschakelen
> * Metrische gegevens weergeven VM
> * Waarschuwingen op basis van diagnostische gegevens maken
> * Pakket-updates beheren
> * Geavanceerde controle instellen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>VM maken

Overzicht van diagnostische gegevens en metrische gegevens in te grijpen, moet u een virtuele machine. Maak eerst een resourcegroep met [az groep maken](/cli/azure/group#create). Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupMonitor* in de *eastus* locatie.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Maak nu een virtuele machine met [az vm maken](https://docs.microsoft.com/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten inschakelen

Als u virtuele Linux-machines opstart, wordt de opstart-extensie voor diagnostische worden vastgelegd opstarten uitvoer en opgeslagen in Azure storage. Deze gegevens kunnen worden gebruikt met virtuele machine opstarten oplossen. Diagnostische gegevens over opstarten zijn niet automatisch ingeschakeld wanneer u een Linux-VM met de Azure CLI maakt.

Voordat u diagnostische gegevens over opstarten inschakelt, moet een opslagaccount worden gemaakt voor het opslaan van de logboeken van de opstartinstallatiekopie. Storage-accounts moeten een globaal unieke naam hebben, tussen 3 en 24 tekens bevatten en mogen alleen cijfers en kleine letters bevatten. Maak een opslagaccount met de [az storage-account maken](/cli/azure/storage/account#create) opdracht. In dit voorbeeld wordt een willekeurige tekenreeks op voor het maken van de naam van een uniek account gebruikt. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Als u diagnostische gegevens over opstarten inschakelt, wordt de URI naar blob storage-container nodig. De volgende opdracht vraagt de storage-account om terug te keren deze URI. De URI-waarde wordt opgeslagen in een variabelenamen *bloburi*, die wordt gebruikt in de volgende stap.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Nu inschakelen diagnostische gegevens over opstarten met [az vm-diagnostische gegevens over opstarten inschakelen](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). De `--storage` waarde is de blob URI verzameld in de vorige stap.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Diagnostische gegevens over opstarten bekijken

Wanneer diagnostische gegevens over opstarten zijn ingeschakeld, is telkens wanneer u stopt en start de virtuele machine, informatie over het opstartproces geschreven naar een logboekbestand. In dit voorbeeld moet u eerst de virtuele machine met ongedaan de [az vm ongedaan](/cli/azure/vm#deallocate) opdracht als volgt:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nu start de virtuele machine met de [az vm start]( /cli/azure/vm#stop) opdracht als volgt:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

U krijgt de diagnostische gegevens van de opstartinstallatiekopie voor *myVM* met de [az vm diagnostische gegevens over opstarten get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) opdracht als volgt:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Metrische gegevens weergeven host

Een Linux-VM heeft toegewezen host in Azure die deze met samenwerkt. Metrische gegevens worden automatisch verzameld voor de host, en kunnen worden weergegeven in de Azure-portal als volgt:

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroupMonitor**, en selecteer vervolgens **myVM** in de lijst met resources.
1. Als u wilt zien hoe de VM-host wordt uitgevoerd, klikt u op **metrische gegevens** op de blade van de virtuele machine en selecteer vervolgens een van de *[Host]* metrische gegevens onder **beschikbare metrische gegevens**.

    ![Metrische gegevens weergeven host](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Installeren van de extensie voor diagnostische gegevens

> [!IMPORTANT]
> Dit document beschrijft versie 2.3 van de extensie voor diagnostische op Linux, dat is afgeschaft. Versie 2.3 worden tot en met 30 juni 2018 ondersteund.
>
> Versie 3.0 van de Linux-extensie voor diagnostische kan in plaats daarvan worden ingeschakeld. Zie voor meer informatie [de documentatie](./diagnostic-extension.md).

De basic host metrische gegevens beschikbaar zijn, maar om meer gedetailleerde en VM-specifieke metrische gegevens ziet, moet u de extensie Azure diagnostics installeren op de virtuele machine. De extensie Azure diagnostics kan aanvullende controle en diagnostische gegevens worden opgehaald van de virtuele machine. U kunt deze maatstaven voor prestaties weergeven en waarschuwingen op basis van de manier waarop de virtuele machine uitvoert. De diagnostische extensie wordt geïnstalleerd via de Azure portal als volgt:

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens **myVM** in de lijst met resources.
1. Klik op **diagnose instellingen**. De lijst ziet u dat *opstarten diagnostics* al uit de vorige sectie zijn ingeschakeld. Klik op het selectievakje voor *basismetrieken*.
1. In de *opslagaccount* sectie, blader naar en selecteer de *mydiagdata [1234]* account gemaakt in de vorige sectie.
1. Klik op de knop **Opslaan**.

    ![Diagnostische metrische gegevens weergeven](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Metrische gegevens weergeven VM

U kunt de metrische gegevens van virtuele machine op dezelfde manier als u de host VM metrische gegevens hebt bekeken bekijken:

1. Klik in de Azure-portal op **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens **myVM** in de lijst met resources.
1. Als u wilt zien hoe de virtuele machine wordt uitgevoerd, klikt u op **metrische gegevens** op de blade virtuele machine en selecteer vervolgens een van de diagnostische gegevens metrische gegevens onder **beschikbare metrische gegevens**.

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

## <a name="manage-package-updates"></a>Pakket-updates beheren

Via updatebeheer kunt u pakket updates en patches voor uw Azure Linux VM's beheren. Rechtstreeks van uw virtuele machine, u kunt snel de status van de beschikbare updates te beoordelen, installatie van vereiste updates plannen en Implementatieresultaten om te controleren of updates met succes zijn toegepast op de virtuele machine bekijken.

Zie [Automation-prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/) voor prijsinformatie.

### <a name="enable-update-management-preview"></a>Schakel het beheer van Update (Preview)

Beheer van de Update voor uw virtuele machine inschakelen

1. Aan de linkerkant van het scherm en selecteer **virtuele machines**.
1. Selecteer een virtuele machine in de lijst.
1. Op het scherm VM in de **Operations** sectie, klikt u op **updatebeheer**. De **updatebeheer inschakelen** scherm wordt geopend.

Er wordt een validatie uitgevoerd om te bepalen of updatebeheer is ingeschakeld voor deze virtuele machine. De validatie bevat controles voor een Log Analytics-werkruimte en het gekoppelde Automation-account en controleert of de oplossing zich in de werkruimte bevindt.

Een werkruimte voor logboekanalyse wordt voor het verzamelen van gegevens die worden gegenereerd door de functies en services zoals updatebeheer gebruikt. De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren. Aanvullende als actie wilt uitvoeren op virtuele machines die updates vereisen, kunt Azure Automation u het uitvoeren van scripts uitvoeren op virtuele machines, zoals downloaden en toepassen van updates.

Tijdens het validatieproces wordt ook gecontroleerd of de VM is ingericht met Microsoft Monitoring Agent (MMA) en Hybrid Worker. Deze agent wordt gebruikt om te communiceren met de VM en om informatie op te vragen over de status van de update. 

Als niet aan deze vereisten wordt voldaan, verschijnt er een banner waarin u de optie voor het inschakelen van de oplossing.

![Banner voor onboardconfiguratie van Updatebeheer](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Klik op de banner om de oplossing in te schakelen. Als een van de volgende vereisten zijn gevonden te zijn verdwenen na de validatie, worden ze automatisch toegevoegd:

* [Log Analytics](../../log-analytics/log-analytics-overview.md)-werkruimte
* [Automatisering](../../automation/automation-offering-get-started.md)
* Een [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) wordt ingeschakeld op de VM.

De **updatebeheer inschakelen** scherm wordt geopend. Configureer de instellingen en klik op **inschakelen**.

![Voor updatebeheer inschakelen](./media/tutorial-monitoring/manage-updates-update-enable.png)

Inschakelen van de oplossing, kan maximaal 15 minuten duren en gedurende deze tijd moet u het browservenster niet sluiten. Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates van de package manager op de virtuele machine met logboekanalyse loopt.
Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

### <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na de **updatebeheer** oplossing is ingeschakeld, de **updatebeheer** scherm wordt weergegeven. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

![Updatestatus bekijken](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Plannen om updates te installeren, een implementatie die volgt op de release-venster voor planning en onderhoud.

Een nieuwe Update-implementatie plannen voor de virtuele machine door te klikken op **schema-update-implementatie** boven aan de **updatebeheer** scherm. Geef de volgende gegevens op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Updates moeten worden uitgesloten** -met deze optie op de namen van pakketten moeten worden uitgesloten van de update opgeven.
* **Schema-instellingen**: u kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd) of een andere tijd opgeven. U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen. Klik op de optie Terugkerend onder Terugkeerpatroon om een terugkerende planning in te stellen.

  ![Scherm met instellingen voor de updateplanning](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Onderhoudsvenster (minuten)**: geef op binnen welke periode de update-implementatie moet plaatsvinden.  Dit zorgt ervoor wijzigingen zijn uitgevoerd binnen de gedefinieerde onderhoudsvensters. 

Nadat u klaar bent met het configureren van de planning, klikt u op de knop **Maken** en gaat u terug naar het statusdashboard.
U ziet dat de **geplande** tabel ziet u het schema voor u gemaakt.

> [!WARNING]
> De virtuele machine wordt opnieuw opgestart nadat updates zijn geïnstalleerd als er voldoende tijd binnen het onderhoudsvenster.

Updatebeheer maakt gebruik van de bestaande package manager op de virtuele machine om pakketten te installeren.

### <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout met een of meer updates in de implementatie, de status is **mislukt**.
Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

![Statusdashboard voor update-implementatie voor specifieke implementatie](./media/tutorial-monitoring/manage-updates-view-results.png)

In **Updateresultaten** tegel is een overzicht van het totale aantal updates en van de Implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van de opgegeven onderhoudsperiode.
* **Geslaagd** -de update is gedownload en geïnstalleerd op de virtuele machine
* **Kan geen** -de update kan niet worden gedownload of geïnstalleerd op de virtuele machine.

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de tegel **Uitvoer** om de taakstroom te bekijken van het runbook dat verantwoordelijk is voor het beheer van de implementatie van de updates op de doel-VM.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="advanced-monitoring"></a>Geavanceerde controle 

U kunt doen meer geavanceerde bewaking van uw virtuele machine met behulp van [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Als u dit nog niet hebt gedaan, kunt u zich aanmelden voor een [gratis proefversie](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) van Operations Management Suite.

Wanneer u toegang tot de OMS-portal hebt, kunt u de werkruimte-id en werkruimtesleutel kunt vinden op de blade instellingen. Vervang < werkruimtesleutel > en < werkruimte-id > met de waarden voor van uw OMS werkruimte en u vervolgens kunt gebruiken **az vm-extensie set** de OMS-extensie toevoegen aan de virtuele machine:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

Op de blade logboek zoeken van de OMS-portal en ziet u *myVM* zoals wat wordt weergegeven in de volgende afbeelding:

![OMS-blade](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt geconfigureerd, gecontroleerd en beheerd updates voor een virtuele machine. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens weergeven host
> * Extensie voor diagnostische gegevens op de virtuele machine inschakelen
> * Metrische gegevens weergeven VM
> * Waarschuwingen op basis van diagnostische gegevens maken
> * Pakket-updates beheren
> * Geavanceerde controle instellen

Ga naar de volgende zelfstudie voor meer informatie over Azure Security Center.

> [!div class="nextstepaction"]
> [VM-beveiliging beheren](./tutorial-azure-security.md)
