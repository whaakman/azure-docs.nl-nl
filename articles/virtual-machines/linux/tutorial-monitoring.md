---
title: 'Zelfstudie: Virtuele Linux-machines bewaken en bijwerken in Azure | Microsoft Docs'
description: In deze zelfstudie leert u hoe u diagnostische gegevens over opstarten en metrische gegevens over prestaties bewaakt en hoe u pakketupdates op een virtuele Linux-machine beheert
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/06/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f8310885ddf7e2229054f36b8fda92b92c1ab01e
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406497"
---
# <a name="tutorial-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Zelfstudie: Een virtuele Linux-machine bewaken en bijwerken in Azure

Om ervoor te zorgen dat uw virtuele machines (VM's) in Azure correct worden uitgevoerd, kunt u diagnostische gegevens over opstarten en maatstaven voor prestaties controleren en pakketupdates beheren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Diagnostische gegevens over opstarten op de virtuele machine inschakelen
> * Diagnostische gegevens over opstarten bekijken
> * Metrische gegevens over de host weergeven
> * De extensie voor diagnostische gegevens inschakelen op de virtuele machine
> * Metrische gegevens over de VM weergeven
> * Waarschuwingen maken op basis van diagnostische gegevens
> * Pakketupdates beheren
> * Wijzigingen en inventaris bewaken
> * Geavanceerde bewaking instellen

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.30 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-vm"></a>VM maken

Om diagnostische gegevens en metrische gegevens in actie te zien, hebt u een virtuele machine nodig. Maak eerst een resourcegroep met [az group create](/cli/azure/group#az-group-create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroupMonitor* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroupMonitor --location eastus
```

Maak een virtuele machine met [az vm create](/cli/azure/vm#az-vm-create). In het volgende voorbeeld wordt een VM gemaakt met de naam *myVM* en worden er in *~/.ssh/* SSH-sleutels gemaakt als deze nog niet bestaan:

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

Voordat u diagnostische gegevens over opstarten inschakelt, moet een opslagaccount worden gemaakt voor het opslaan van de opstartlogboeken. Opslagaccounts moeten een globaal unieke naam hebben, tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. Maak een opslagaccount met de opdracht [az storage account create](/cli/azure/storage/account#az-storage-account-create). In dit voorbeeld wordt een willekeurige tekenreeks gebruikt voor het maken van een unieke naam voor het opslagaccount.

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

Schakel diagnostische gegevens nu in met [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az-vm-boot-diagnostics-enable). De waarde `--storage` is de blob-URI die in de vorige stap is opgehaald.

```azurecli-interactive
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```

## <a name="view-boot-diagnostics"></a>Diagnostische gegevens over opstarten bekijken

Wanneer diagnostische gegevens over opstarten zijn ingeschakeld, wordt telkens wanneer u de virtuele machine stopt en start informatie over het opstartproces naar een logboekbestand geschreven. In dit voorbeeld moet u eerst de toewijzing van de virtuele machine als volgt ongedaan maken met de opdracht [az vm deallocate](/cli/azure/vm#az-vm-deallocate):

```azurecli-interactive
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Start de virtuele machine nu als volgt met de opdracht [az vm start]( /cli/azure/vm#az-vm-stop):

```azurecli-interactive
az vm start --resource-group myResourceGroupMonitor --name myVM
```

U kunt de diagnostische gegevens over opstarten voor *myVM* als volgt ophalen met de opdracht [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az-vm-boot-diagnostics-get-boot-log):

```azurecli-interactive
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```

## <a name="view-host-metrics"></a>Metrische gegevens over de host weergeven

Een Linux-VM heeft een toegewezen host in Azure die met deze VM samenwerkt. Metrische gegevens worden automatisch verzameld voor de host, en kunnen als volgt worden weergegeven in Azure Portal:

1. Selecteer in Azure Portal de optie **Resourcegroepen**, kies **myResourceGroupMonitor** en selecteer vervolgens **myVM** in de lijst met resources.
1. Als u wilt zien hoe de host-VM presteert, selecteert u **Metrische gegevens** in het VM-venster en kiest u vervolgens een van de metrische gegevens voor *[Host]* onder **Beschikbare metrische gegevens**.

    ![Metrische gegevens over de host weergeven](./media/tutorial-monitoring/monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>De extensie voor diagnostische gegevens installeren

De metrische basisgegevens over de host zijn beschikbaar, maar als u meer gedetailleerde en VM-specifieke metrische gegevens wilt bekijken, moet u de Azure Diagnostics-extensie op de virtuele machine installeren. Met de Azure Diagnostics-extensie kunnen aanvullende bewakings- en diagnostische gegevens worden opgehaald van de virtuele machine. U kunt deze maatstaven voor prestaties weergeven en waarschuwingen maken op basis van de manier waarop de virtuele machine presteert. De Azure Diagnostics-extensie wordt als volgt geïnstalleerd via Azure Portal:

1. Kies in Azure Portal de optie **Resourcegroepen**, selecteer **myResourceGroupMonitor** en selecteer **myVM** in de lijst met resources.
1. Selecteer **Diagnose-instellingen**. Kies in de vervolgkeuzelijst *Een opslagaccount kiezen* het account *mydiagdata [1234]* (als dit nog niet is geselecteerd) dat in de vorige sectie is gemaakt.
1. Selecteer de knop **Bewaking op gastniveau inschakelen**.

    ![Metrische gegevens over de diagnose weergeven](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Metrische gegevens over de VM weergeven

U kunt de metrische gegevens over de virtuele machine op dezelfde manier weergeven als u de metrische gegevens over de host-VM hebt weergegeven:

1. Kies in Azure Portal de optie **Resourcegroepen**, selecteer **myResourceGroupMonitor** en selecteer **myVM** in de lijst met resources.
1. Als u wilt zien hoe de VM presteert, selecteert u **Metrische gegevens** in het VM-venster en selecteert u vervolgens een van de metrische gegevens voor *[Guest]* onder **Beschikbare metrische gegevens**.

    ![Metrische gegevens over de VM weergeven](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Waarschuwingen maken

U kunt waarschuwingen maken op basis van specifieke maatstaven voor prestaties. Waarschuwingen kunnen worden gebruikt om u een melding te sturen wanneer het gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt of de beschikbare vrije schijfruimte onder een bepaalde hoeveelheid komt. Waarschuwingen worden weergegeven in Azure Portal of kunnen worden verzonden via e-mail. U kunt ook Azure Automation-runbooks of Azure Logic Apps activeren als reactie op het genereren van waarschuwingen.

In het volgende voorbeeld wordt een waarschuwing gemaakt voor het gemiddelde CPU-gebruik.

1. Selecteer in Azure Portal de optie **Resourcegroepen**, selecteer **myResourceGroupMonitor** en selecteer vervolgens **myVM** in de lijst met resources.
2. Selecteer **Waarschuwingen (klassiek)**, kies **Waarschuwing voor metrische gegevens toevoegen (klassiek)** bovenin het waarschuwingenvenster.
3. Geef een **Naam** op voor de waarschuwing, zoals *myAlertRule*
4. Als u een waarschuwing wilt activeren wanneer het CPU-percentage gedurende vijf minuten 1.0 overschrijdt, laat u alle overige standaardwaarden geselecteerd.
5. Schakel desgewenst het selectievakje voor *E-mailadressen van eigenaren, bijdragers en lezers* in om een e-mailmelding te verzenden. Standaard wordt een melding in de portal weergegeven.
6. Selecteer de knop **OK**.

## <a name="manage-package-updates"></a>Pakketupdates beheren

Met Updatebeheer kunt u updates en patches beheren voor een Azure Linux-VM.
U kunt rechtstreeks vanuit uw VM de status van de beschikbare updates beoordelen, de installatie van vereiste updates plannen en de implementatieresultaten bekijken om te controleren of updates correct zijn toegepast op de VM.

Zie [Automation-prijzen voor updatebeheer](https://azure.microsoft.com/pricing/details/automation/) voor prijsinformatie.

### <a name="enable-update-management"></a>Updatebeheer inschakelen

Updatebeheer inschakelen voor de VM:

1. Selecteer aan de linkerkant van het scherm **Virtuele machines**.
2. Selecteer een VM in de lijst.
3. Selecteer in de sectie **Bewerkingen** van het VM-scherm de optie **Updatebeheer**. Het scherm **Updatebeheer inschakelen** wordt weergegeven.

Er wordt een validatie uitgevoerd om te bepalen of updatebeheer is ingeschakeld voor deze virtuele machine.
De validatie bevat controles voor een Log Analytics-werkruimte en het gekoppelde Automation-account en controleert of de oplossing zich in de werkruimte bevindt.

Een [Log Analytics](../../log-analytics/log-analytics-overview.md)-werkruimte wordt gebruikt om gegevens te verzamelen die worden gegenereerd door functies en services zoals Updatebeheer.
De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren.
Als u aanvullende bewerkingen wilt uitvoeren op virtuele machines die updates vereisen, biedt Azure Automation de mogelijkheid runbooks uit te voeren met VM's, zoals updates downloaden en toepassen.

Tijdens het validatieproces wordt ook gecontroleerd of de virtuele machine is ingericht met Microsoft Monitoring Agent (MMA) en Automation Hybrid Runbook Worker.
Deze agent wordt gebruikt om te communiceren met de VM en om informatie op te vragen over de status van de update.

Kies de Log Analytics-werkruimte en het Automation-account en selecteer **Inschakelen** om de oplossing in te schakelen. Het duurt maximaal 15 minuten om de oplossing in te schakelen.

Als een van de volgende vereiste onderdelen ontbreekt na de onboarding, wordt dit automatisch toegevoegd:

* [Log Analytics](../../log-analytics/log-analytics-overview.md)-werkruimte
* [Automation](../../automation/automation-offering-get-started.md)
* Een [Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md) wordt ingeschakeld op de virtuele machine.

Het scherm **Updatebeheer** wordt geopend. Configureer de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt, en selecteer **Inschakelen**. Als de velden lichtgrijs zijn, betekent dit dat een andere automatiseringsoplossing is ingeschakeld voor de virtuele machine en dat dezelfde werkruimte en hetzelfde Automation-account moeten worden gebruikt.

![Oplossing voor updatebeheer inschakelen](./media/tutorial-monitoring/manage-updates-update-enable.png)

Het inschakelen van de oplossing kan maximaal 15 minuten duren. Gedurende deze tijd mag u het browservenster niet sluiten. Nadat de oplossing is ingeschakeld, wordt informatie over ontbrekende updates op de VM naar Log Analytics verzonden. Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

### <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven. Nadat de evaluatie van updates is voltooid, ziet u een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

 ![Updatestatus bekijken](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie na uw release-planning en servicevenster. U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Als u een nieuwe update-implementatie voor de VM wilt plannen, selecteert u **Update-implementatie plannen** bovenin het scherm **Updatebeheer**. Geef de volgende gegevens op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Updateclassificatie**: selecteer de typen software die de update-implementatie moet opnemen in de implementatie. De classificatietypen zijn:
  * Essentiële en beveiligingsupdates
  * Andere Updates
* **Updates die moeten worden uitgesloten**: u kunt een lijst met namen van pakketten opgeven die moeten worden overgeslagen tijdens de update-implementatie. Pakketnamen bieden ondersteuning voor jokertekens (zoals \*kernal\*).

  ![Scherm met instellingen voor de updateplanning](./media/tutorial-monitoring/manage-updates-exclude-linux.png)

* **Schema-instellingen**: u kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd) of een andere tijd opgeven.
  U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen. Selecteer de optie Terugkerend onder Terugkeerpatroon als u een terugkerend schema wilt instellen.

  ![Scherm met instellingen voor de updateplanning](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Onderhoudsvenster (minuten)**: geef op binnen welke periode de update-implementatie moet plaatsvinden. Dit zorgt ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde servicevensters.

Wanneer u de planning hebt geconfigureerd, selecteert u de knop **Maken** en gaat u terug naar het statusdashboard.
U ziet dat de tabel **Gepland** de implementatieplanning weergeeft die u hebt gemaakt.

> [!WARNING]
> Voor updates waarvoor opnieuw opstarten is vereist, wordt de VM automatisch opnieuw opgestart.

### <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout is opgetreden met één of meer updates in de implementatie, wordt de status **Gedeeltelijk mislukt**.
Selecteer de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

![Statusdashboard voor update-implementatie voor specifieke implementatie](./media/tutorial-monitoring/manage-updates-view-results.png)

Op de tegel **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van de opgegeven onderhoudsperiode.
* **Geslaagd**: de update is voltooid.
* **Mislukt**: de update is mislukt.

Selecteer **Alle logboeken** als u alle logboekvermeldingen wilt zien die tijdens de implementatie zijn gemaakt.

Selecteer de tegel **Uitvoer** als u de taakstroom wilt bekijken van het runbook dat verantwoordelijk is voor het beheer van de implementatie van de updates op de doel-VM.

Selecteer **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="monitor-changes-and-inventory"></a>Wijzigingen en inventaris bewaken

U kunt inventarisgegevens verzamelen en weergeven voor software, bestanden, Linux-daemons, Windows-services en Windows-registersleutels op uw computers. Door de configuraties van uw computer bij te houden, is het voor u gemakkelijker vast te stellen waar in uw omgeving zich operationele problemen voordoen, en kunt u beter beoordelen wat de status van uw computers is.

### <a name="enable-change-and-inventory-management"></a>Wijzigings- en inventarisbeheer inschakelen

Wijzigings- en inventarisbeheer inschakelen voor de VM:

1. Selecteer aan de linkerkant van het scherm **Virtuele machines**.
2. Selecteer een VM in de lijst.
3. Selecteer in de sectie **Bewerkingen** van het VM-scherm de optie **Inventaris** of **Wijzigingen bijhouden**. Het scherm **Wijzigingen bijhouden en inventaris inschakelen** wordt geopend.

Configureer de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt, en selecteer **Inschakelen**. Als de velden lichtgrijs zijn, betekent dit dat een andere automatiseringsoplossing is ingeschakeld voor de virtuele machine en dat dezelfde werkruimte en hetzelfde Automation-account moeten worden gebruikt. Hoewel de oplossingen afzonderlijk worden weergegeven in het menu, is het dezelfde oplossing. Als u er één inschakelt, worden beide ingeschakeld voor de VM.

![Bijhouden van wijzigingen en inventaris inschakelen](./media/tutorial-monitoring/manage-inventory-enable.png)

Nadat de oplossing is ingeschakeld, kan het enige tijd duren voordat de gegevens zichtbaar zijn terwijl de inventaris wordt verzameld op de VM.

### <a name="track-changes"></a>Wijzigingen bijhouden

Selecteer op de VM de optie **Wijzigingen bijhouden** onder **BEWERKINGEN**. Selecteer **Instellingen bewerken**. De pagina **Wijzigingen bijhouden** wordt weergegeven. Selecteer het type instelling dat u wilt bijhouden en selecteer **+ Toevoegen** om de instellingen te configureren. De beschikbare optie voor Linux is **Linux-bestanden**

Zie [Problemen met wijzigingen op een VM oplossen](../../automation/automation-tutorial-troubleshoot-changes.md) voor gedetailleerde informatie over Wijzigingen bijhouden

### <a name="view-inventory"></a>Inventaris weergeven

Selecteer op de VM de optie **Inventaris** onder **BEWERKINGEN**. Op het tabblad **Software** wordt een lijst weergegeven met de software die is gevonden. De algemene details voor elke softwarerecord worden in de tabel weergegeven. Deze details omvatten de naam, versie, uitgever en laatste tijdstip van vernieuwen voor de software.

![Inventaris weergeven](./media/tutorial-monitoring/inventory-view-results.png)

### <a name="monitor-activity-logs-and-changes"></a>Activiteitenlogboeken en wijzigingen bewaken

Selecteer **Verbinding met het activiteitenlogboek beheren** op de pagina **Wijzigingen bijhouden** op de virtuele machine. Hiermee opent u de pagina **Azure-activiteitenlogboek**. Selecteer **Verbinden** om Wijzigingen bijhouden te verbinden met het Azure-activiteitenlogboek voor uw virtuele machine.

Terwijl deze instelling is ingeschakeld, gaat u naar de pagina **Overzicht** voor de virtuele machine en selecteert u **Stoppen** om de virtuele machine te stoppen. Wanneer u daarom wordt gevraagd, selecteert u **Ja** om de virtuele machine te stoppen. Wanneer deze toewijzing ongedaan is gemaakt, selecteert u **Starten** om de virtuele machine opnieuw op te starten.

Wanneer een virtuele machine wordt gestart en gestopt, wordt een gebeurtenis geregistreerd in het activiteitenlogboek. Ga terug naar de pagina **Wijzigingen bijhouden**. Selecteer het tabblad **Gebeurtenissen** onderaan op de pagina. Na een tijdje worden de gebeurtenissen weergegeven in de grafiek en in de tabel. Elke gebeurtenis kan worden geselecteerd om gedetailleerde informatie over de gebeurtenis weer te geven.

![Wijzigingen in het activiteitenlogboek weergeven](./media/tutorial-monitoring/manage-activitylog-view-results.png)

De grafiek toont wijzigingen die in de loop der tijd hebben plaatsgevonden. Nadat u een verbinding met een Azure-activiteitenlogboek hebt toegevoegd, toont de lijngrafiek bovenaan gebeurtenissen uit het Azure-actitviteitenlogboek. Elke rij in het staafdiagram vertegenwoordigt een ander type wijziging dat kan worden gevolgd. Deze typen zijn daemons, bestanden en software van Linux. Het tabblad met wijzigingen bevat details over de wijzigingen in de visualisatie. Deze worden getoond in aflopende volgorde vanaf het moment waarop de wijziging optrad (meest recente eerst).

## <a name="advanced-monitoring"></a>Geavanceerde controle

U kunt geavanceerdere bewaking van de VM uitvoeren met behulp van de oplossingen zoals Updatebeheer en Wijzigingen en inventaris bewaken, mogelijk gemaakt met [Azure Automation](../../automation/automation-intro.md).

Wanneer u toegang hebt tot de Log Analytics-werkruimte, kunt u de werkruimtesleutel en werkruimte-id vinden door onder **INSTELLINGEN** de optie **Geavanceerde instellingen** te selecteren. Vervang \<werkruimtesleutel \> en \< werkruimte-id\> door de waarden van uw Log Analytics-werkruimte. U kunt dan **az vm extension set** gebruiken om de extensie toe te voegen aan de VM:

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

Na enkele minuten ziet u de nieuwe VM in de Log Analytics-werkruimte.

![Log Analytics](./media/tutorial-monitoring/tutorial-monitor-oms.png)

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
> * Wijzigingen en inventaris bewaken
> * Geavanceerde bewaking instellen

Ga naar de volgende zelfstudie voor meer informatie over Azure Security Center.

> [!div class="nextstepaction"]
> [VM-beveiliging beheren](./tutorial-azure-security.md)
