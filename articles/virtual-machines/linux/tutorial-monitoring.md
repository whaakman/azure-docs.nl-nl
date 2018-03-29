---
title: Virtuele Linux-machines bewaken en bijwerken in Azure | Microsoft Docs
description: Informatie over het bewaken van diagnostische gegevens over opstarten, maatstaven voor prestaties en het beheren van pakketupdates op een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9ffd36da535a2e5ac4a355f429394dc4209348b7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Een virtuele Linux-machine bewaken en bijwerken in Azure

Om ervoor te zorgen dat uw virtuele machines (VM's) in Azure correct worden uitgevoerd, kunt u diagnostische gegevens over opstarten en maatstaven voor prestaties controleren en pakketupdates beheren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens over de host weergeven
> * De extensie voor diagnostische gegevens inschakelen op de virtuele machine
> * Metrische gegevens over de VM weergeven
> * Waarschuwingen maken op basis van diagnostische gegevens
> * Pakketupdates beheren
> * Geavanceerde bewaking instellen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>VM maken

Om diagnostische gegevens en metrische gegevens in actie te zien, hebt u een virtuele machine nodig. Maak eerst een resourcegroep met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupMonitor* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Maak een virtuele machine met [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create). In het volgende voorbeeld wordt een VM met de naam *myVM* gemaakt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Diagnostische gegevens over opstarten inschakelen

Als virtuele Linux-machines worden opgestart, wordt uitvoer vastgelegd in de extensie voor diagnostische gegevens over opstarten en opgeslagen in Azure Storage. Deze gegevens kunnen worden gebruikt om opstartproblemen met virtuele machines op te lossen. Diagnostische gegevens over opstarten zijn niet automatisch ingeschakeld wanneer u een Linux-VM met de Azure CLI maakt.

Voordat u diagnostische gegevens over opstarten inschakelt, moet een opslagaccount worden gemaakt voor het opslaan van de opstartlogboeken. Opslagaccounts moeten een globaal unieke naam hebben, tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. Maak een opslagaccount met de opdracht [az storage account create](/cli/azure/storage/account#az_storage_account_create). In dit voorbeeld wordt een willekeurige tekenreeks gebruikt voor het maken van een unieke naam voor het opslagaccount. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Als u diagnostische gegevens over opstarten inschakelt, is de URI naar de opslagcontainer voor de blob nodig. Met de volgende opdracht wordt een query uitgevoerd op de opslagaccount om deze URI op te halen. De URI-waarde wordt opgeslagen in een variabele met de naam *bloburi*, die in de volgende stap wordt gebruikt.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Schakel diagnostische gegevens nu in met [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). De waarde `--storage` is de blob-URI die in de vorige stap is opgehaald.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Diagnostische gegevens over opstarten bekijken

Wanneer diagnostische gegevens over opstarten zijn ingeschakeld, wordt telkens wanneer u de virtuele machine stopt en start informatie over het opstartproces naar een logboekbestand geschreven. In dit voorbeeld moet u eerst de toewijzing van de virtuele machine als volgt ongedaan maken met de opdracht [az vm deallocate](/cli/azure/vm#az_vm_deallocate):

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Start de virtuele machine nu als volgt met de opdracht [az vm start]( /cli/azure/vm#az_vm_stop):

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

U kunt de diagnostische gegevens over opstarten voor *myVM* als volgt ophalen met de opdracht [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log):

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Metrische gegevens over de host weergeven

Een Linux-VM heeft een toegewezen host in Azure die met deze VM samenwerkt. Metrische gegevens worden automatisch verzameld voor de host, en kunnen als volgt worden weergegeven in Azure Portal:

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroupMonitor** en selecteer vervolgens **myVM** in de lijst met resources.
1. Als u wilt zien hoe de host-VM wordt uitgevoerd, klikt u op **Metrische gegevens** op de blade van de virtuele machine en selecteert u vervolgens een van de metrische gegevens over *[Host]* onder **Beschikbare metrische gegevens**.

    ![Metrische gegevens over de host weergeven](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>De extensie voor diagnostische gegevens installeren

> [!IMPORTANT]
> In dit document wordt versie 2.3 van de Linux Diagnostic-extensie beschreven, die uit gebruik is genomen. Versie 2.3 wordt ondersteund tot en met 30 juni 2018.
>
> In plaats daarvan kan versie 3.0 van de Linux Diagnostic-extensie worden ingeschakeld. Raadpleeg de [documentatie](./diagnostic-extension.md) voor meer informatie.

De metrische basisgegevens over de host zijn beschikbaar, maar als u meer gedetailleerde en VM-specifieke metrische gegevens wilt bekijken, moet u de Azure Diagnostics-extensie op de virtuele machine installeren. Met de Azure Diagnostics-extensie kunnen aanvullende bewakings- en diagnostische gegevens worden opgehaald van de virtuele machine. U kunt deze maatstaven voor prestaties weergeven en waarschuwingen maken op basis van de manier waarop de virtuele machine presteert. De Azure Diagnostics-extensie wordt als volgt geïnstalleerd via Azure Portal:

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
1. Klik op **Diagnose-instellingen**. In de lijst ziet u dat *Diagnostische gegevens over opstarten* al is ingeschakeld in de vorige sectie. Klik op het selectievakje voor *Basismetrieken*.
1. Blader in de sectie *Opslagaccount* naar de account *mydiagdata [1234]* die in de vorige sectie is gemaakt en selecteer deze.
1. Klik op de knop **Opslaan**.

    ![Metrische gegevens over de diagnose weergeven](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Metrische gegevens over de VM weergeven

U kunt de metrische gegevens over de virtuele machine op dezelfde manier weergeven als u de metrische gegevens over de host-VM hebt weergegeven:

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
1. Als u wilt zien hoe de VM wordt uitgevoerd, klikt u op **Metrische gegevens** op de blade van de virtuele machine en selecteert u vervolgens een van de metrische gegevens onder **Beschikbare metrische gegevens**.

    ![Metrische gegevens over de VM weergeven](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Waarschuwingen maken

U kunt waarschuwingen maken op basis van specifieke maatstaven voor prestaties. Waarschuwingen kunnen worden gebruikt om u een melding te sturen wanneer het gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt of de beschikbare vrije schijfruimte onder een bepaalde hoeveelheid komt. Waarschuwingen worden weergegeven in Azure Portal of kunnen worden verzonden via e-mail. U kunt ook Azure Automation-runbooks of Azure Logic Apps activeren als reactie op het genereren van waarschuwingen.

In het volgende voorbeeld wordt een waarschuwing gemaakt voor het gemiddelde CPU-gebruik.

1. Klik in Azure Portal op **Resourcegroepen**, selecteer **myResourceGroup** en selecteer vervolgens **myVM** in de lijst met resources.
2. Klik op de VM-blade op **Waarschuwingsregels** en klik vervolgens boven aan de waarschuwingenblade op **Waarschuwing voor metrische gegevens toevoegen**.
4. Geef een **Naam** op voor de waarschuwing, zoals *myAlertRule*
5. Als u een waarschuwing wilt activeren wanneer het CPU-percentage gedurende vijf minuten 1.0 overschrijdt, laat u alle overige standaardwaarden geselecteerd.
6. Schakel desgewenst het selectievakje voor *E-mailadressen van eigenaren, bijdragers en lezers* in om een e-mailmelding te verzenden. Standaard wordt een melding in de portal weergegeven.
7. Klik op de knop **OK**.

## <a name="manage-package-updates"></a>Pakketupdates beheren

Via updatebeheer kunt u pakketupdates en patches beheren voor uw Azure Linux-VM's. U kunt rechtstreeks vanuit uw VM de status van de beschikbare updates beoordelen, de installatie van vereiste updates plannen en de implementatieresultaten bekijken om te controleren of updates correct zijn toegepast op de VM.

Zie [Automation-prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/) voor prijsinformatie.

### <a name="enable-update-management-preview"></a>Updatebeheer inschakelen (preview)

Updatebeheer inschakelen voor uw VM

1. Selecteer aan de linkerkant van het scherm **Virtuele machines**.
1. Selecteer een VM in de lijst.
1. Klik op het VM-scherm in de sectie **Bewerkingen** op **Updatebeheer**. Het scherm **Updatebeheer inschakelen** wordt weergegeven.

Er wordt een validatie uitgevoerd om te bepalen of updatebeheer is ingeschakeld voor deze virtuele machine. De validatie bevat controles voor een Log Analytics-werkruimte en het gekoppelde Automation-account en controleert of de oplossing zich in de werkruimte bevindt.

Een Log Analytics-werkruimte wordt gebruikt om gegevens te verzamelen die worden gegenereerd door functies en services zoals Updatebeheer. De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren. Als u aanvullende bewerkingen wilt uitvoeren op virtuele machines die updates vereisen, biedt Azure Automation de mogelijkheid scripts uit te voeren voor VM's, zoals updates downloaden en toepassen.

Tijdens het validatieproces wordt ook gecontroleerd of de virtuele machine is ingericht met Microsoft Monitoring Agent (MMA) en Hybrid Worker. Deze agent wordt gebruikt om te communiceren met de VM en om informatie op te vragen over de status van de update. 

Als niet aan deze vereisten wordt voldaan, verschijnt er een banner waarin u de optie krijgt de oplossing in te schakelen.

![Banner voor onboardconfiguratie van Updatebeheer](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Klik op de banner om de oplossing in te schakelen. Als een van de volgende vereiste onderdelen ontbreekt na de validatie, wordt dit automatisch toegevoegd:

* [Log Analytics](../../log-analytics/log-analytics-overview.md)-werkruimte
* [Automatisering](../../automation/automation-offering-get-started.md)
* Een [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) wordt ingeschakeld op de VM.

Het scherm **Updatebeheer inschakelen** wordt weergegeven. Configureer de instellingen en klik op **Inschakelen**.

![Oplossing voor updatebeheer inschakelen](./media/tutorial-monitoring/manage-updates-update-enable.png)

Het inschakelen van de oplossing kan maximaal 15 minuten duren en gedurende deze tijd moet u het browservenster niet sluiten. Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates uit Pakketbeheer op de VM naar Log Analytics verzonden.
Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

### <a name="view-update-assessment"></a>Update-evaluatie bekijken

Nadat de oplossing van **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

![Updatestatus bekijken](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie volgens uw releaseplanning en onderhoudsvenster.

Plan een nieuwe update-implementatie voor de VM door te klikken op **Update-implementatie plannen** boven aan het scherm **Updatebeheer**. Geef de volgende gegevens op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Updates die moeten worden uitgesloten**: selecteer deze optie om de namen van pakketten op te geven die moeten worden uitgesloten van de update.
* **Schema-instellingen**: u kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd) of een andere tijd opgeven. U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen. Klik op de optie Terugkerend onder Terugkeerpatroon om een terugkerende planning in te stellen.

  ![Scherm met instellingen voor de updateplanning](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Onderhoudsvenster (minuten)**: geef op binnen welke periode de update-implementatie moet plaatsvinden.  Dit zorgt ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde onderhoudsvensters. 

Nadat u klaar bent met het configureren van de planning, klikt u op de knop **Maken** en gaat u terug naar het statusdashboard.
U ziet dat de tabel **Gepland** de implementatieplanning weergeeft die u hebt gemaakt.

> [!WARNING]
> De virtuele machine wordt automatisch opnieuw opgestart nadat updates zijn geïnstalleerd als er voldoende tijd is binnen het onderhoudsvenster.

Updatebeheer maakt gebruik van de bestaande pakketbeheerfunctie op de virtuele machine om pakketten te installeren.

### <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout is opgetreden met één of meer updates in de implementatie, wordt de status op **Mislukt** ingesteld.
Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

![Statusdashboard voor update-implementatie voor specifieke implementatie](./media/tutorial-monitoring/manage-updates-view-results.png)

Op de tegel **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van de opgegeven onderhoudsperiode.
* **Geslaagd**: de update is gedownload en geïnstalleerd op de virtuele machine
* **Mislukt**: de update kan niet worden gedownload of geïnstalleerd op de virtuele machine.

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de tegel **Uitvoer** om de taakstroom te bekijken van het runbook dat verantwoordelijk is voor het beheer van de implementatie van de updates op de doel-VM.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="advanced-monitoring"></a>Geavanceerde controle 

U kunt een meer geavanceerde bewaking toepassen op uw virtuele machine met behulp van [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). U kunt u zich aanmelden voor een [gratis proefversie](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) van Operations Management Suite als u dit nog niet hebt gedaan.

Wanneer u toegang tot de OMS-portal hebt, kunt u de werkruimtesleutel en werkruimte-id vinden op de blade Instellingen. Vervang < werkruimtesleutel > en < werkruimte-id > door de waarden van uw OMS-werkruimte. U kunt dan **az vm extension set** gebruiken om de OMS-extensie toe te voegen aan de virtuele machine:

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

Op de blade Zoeken in logboeken van de OMS-portal ziet u *myVM* zoals wordt weergegeven op de volgende afbeelding:

![OMS-blade](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u updates voor een virtuele machine geconfigureerd, gecontroleerd en beheerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens over de host weergeven
> * De extensie voor diagnostische gegevens inschakelen op de virtuele machine
> * Metrische gegevens over de VM weergeven
> * Waarschuwingen maken op basis van diagnostische gegevens
> * Pakketupdates beheren
> * Geavanceerde bewaking instellen

Ga naar de volgende zelfstudie voor meer informatie over Azure Security Center.

> [!div class="nextstepaction"]
> [VM-beveiliging beheren](./tutorial-azure-security.md)
