---
title: "Azure Batch-pools van aangepaste installatiekopieën inrichten | Microsoft Docs"
description: "U kunt een Batch toepassingen van een aangepaste installatiekopie die u wilt inrichten rekenknooppunten die de software en gegevens die u nodig hebt voor uw toepassing bevatten. Aangepaste installatiekopieën zijn een efficiënte manier voor het configureren van rekenknooppunten om uit te voeren van uw Batch-workloads."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 3d655766b4f2a5efb0c8c29ffa81a89f84b3e17c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Een aangepaste installatiekopie gebruiken voor een pool van virtuele machines maken

Wanneer u een pool van virtuele machines in Azure Batch maakt, geeft u de installatiekopie van een virtuele machine (VM) waarmee het besturingssysteem voor elk rekenknooppunt in de groep. U kunt een pool van virtuele machines maken met behulp van een installatiekopie van een Azure Marketplace of door een aangepaste VHD-installatiekopie die u hebt voorbereid. Wanneer u een aangepaste installatiekopie opgeeft, hebt u controle over de manier waarop het besturingssysteem is geconfigureerd op het moment dat elk rekenknooppunt is ingericht. Uw aangepaste installatiekopie kan ook toepassingen en referentiegegevens die beschikbaar op het rekenknooppunt zijn zodra deze is ingericht.

Met een aangepaste installatiekopie kunt u bij het ophalen van uw pool tijd besparen rekenknooppunten gereed voor gebruik van uw Batch-werkbelasting. Terwijl u kunt altijd een Azure Marketplace-installatiekopie gebruiken en software op elk rekenknooppunt installeren nadat deze zijn ingericht, is het mogelijk dat deze aanpak minder efficiënt dan met een aangepaste installatiekopie. 

Enkele redenen met een aangepaste installatiekopie die is geconfigureerd voor uw scenario zijn hoeven:

- **Configureren van het besturingssysteem (OS)** geen speciale configuratie van het besturingssysteem voor de aangepaste installatiekopie kan worden uitgevoerd. 
- **Grote toepassingen installeren.** Toepassingen installeren op een aangepaste installatiekopie is efficiënter dan installeren op elk rekenknooppunt nadat deze is ingericht.
- **Kopiëren van grote hoeveelheden gegevens.** Als de gegevens gekopieerd naar de aangepaste installatiekopie, moet deze slechts één keer plaats op elk rekenknooppunt, tijd en bandbreedte besparen worden gekopieerd.
- **Start opnieuw op de virtuele machine tijdens de installatie.** Opnieuw opstarten van de virtuele machine kan een tijdrovend proces zijn, met name als u een aantal rekenknooppunten.

## <a name="prerequisites"></a>Vereisten

- **Een Batch-account is gemaakt met de gebruikerabonnement groep toewijzing-modus.** Voor het gebruik van een aangepaste installatiekopie voor het inrichten van virtuele Machine van toepassingen, uw Batch-account te maken met abonnement van de gebruiker [groep toewijzing modus](batch-api-basics.md#pool-allocation-mode). In deze modus worden Batch-pools toegewezen binnen het abonnement waarin het account zich bevindt. Zie de [Account](batch-api-basics.md#account) in sectie [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md) voor informatie over het instellen van de modus van de toewijzing van toepassingen wanneer u een Batch-account maakt.

- Een **Azure Storage-account.** Voor het maken van een pool van virtuele machines met een aangepaste installatiekopie, moet u een standaard, algemene Azure Storage-account in hetzelfde abonnement en dezelfde regio. Als u een aangepaste installatiekopie van een virtuele machine in Azure maakt, wordt u de installatiekopie kopiëren naar het opslagaccount waarin de besturingssysteemschijf van de VM zich bevindt en u hoeft niet te maken van een afzonderlijke opslagaccount. 
    
## <a name="prepare-a-custom-image"></a>Een aangepaste installatiekopie voorbereiden

Als u een aangepaste installatiekopie voorbereiden voor gebruik met Batch, moet u een bestaande installatie van Linux of Windows te generaliseren. De installatie van een besturingssysteem te generaliseren verwijdert systeemspecifieke gegevens. Het resultaat is een installatiekopie die kan worden geïnstalleerd op andere computers of virtuele machines.  

> [!IMPORTANT]
> Batch ondersteunt momenteel geen gebruik van Azure installatiekopieën voor het inrichten van een groep beheerd. De aangepaste installatiekopie die u gebruikt voor het inrichten van een groep moet worden opgeslagen in Azure Storage. 
>
> Bij het voorbereiden van uw aangepaste installatiekopie, houd er rekening mee houden de volgende punten:
> - Zorg ervoor dat de basisinstallatiekopie voor het besturingssysteem die u gebruikt om uw Batch-pools in te richten geen vooraf geïnstalleerde Azure-extensies bevat, zoals de extensie Aangepast script. Als de installatiekopie een vooraf geïnstalleerde extensie bevat, kunnen er binnen Azure problemen optreden bij het implementeren van de VM.
> - Zorg ervoor dat in de basisinstallatiekopie van het besturingssysteem die u opgeeft, gebruik wordt gemaakt van het reguliere tijdelijke station. De Batch-knooppuntagent verwacht momenteel namelijk het reguliere tijdelijke station.
>
>

Als een aangepaste installatiekopie kunt u eventuele bestaande voorbereide installatiekopie van een Windows- of Linux. Bijvoorbeeld als u een lokale installatiekopie gebruiken wilt, klikt u vervolgens de installatiekopie uploadt naar een Azure-opslagaccount die zich in hetzelfde abonnement en dezelfde regio bevinden als uw Batch-account met [AzCopy](../storage/storage-use-azcopy.md) of een ander hulpprogramma voor het uploaden.

U kunt ook een aangepaste installatiekopie van een nieuwe of bestaande Azure-virtuele machine voorbereiden. Als u een nieuwe virtuele machine maakt, kunt u een Azure Marketplace-installatiekopie gebruiken als de basisinstallatiekopie voor uw aangepaste installatiekopie en deze aanpassen. Voor het aanpassen van de basisinstallatiekopie van een virtuele machine in Azure maken en uw toepassingen of gegevens toevoegen aan deze. Generaliseer vervolgens de virtuele machine om te fungeren als de aangepaste installatiekopie en opslaan in Azure Storage. 

Als u een aangepaste installatiekopie van een virtuele machine in Azure, de volgende stappen uit:

1. Maak een **onbeheerde** Azure VM van de installatiekopie van een Azure Marketplace. Azure Marketplace afbeeldingen bevat voor zowel [Windows](../virtual-machines/windows/quick-create-portal.md) en [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    Zorg ervoor dat u selecteert bij stap 3 van het proces voor het maken van VM **Nee** voor **opslag: gebruik beheerd schijven** optie. Ook Noteer de naam van het opslagaccount voor de besturingssysteemschijf van de VM, omdat dit opslagaccount ook waar uw aangepaste installatiekopie wordt opgeslagen in Azure:

    ![Een niet-beheerde virtuele machine maken en noteer de opslagaccountnaam](media/batch-custom-images/vm-create-storage.png)
 
2. Het proces van het maken van uw virtuele machine te voltooien en wacht totdat deze door Azure worden toegewezen. Hier volgt een afbeelding die laat zien van een virtuele machine in de Azure portal in de actieve status:

    ![Een virtuele machine maken vanuit een marketplace-installatiekopie](media/batch-custom-images/vm-status-running.png)

3. Zodra de virtuele machine wordt uitgevoerd, verbinding maken via RDP (voor Windows) of SSH (voor Linux). Alle benodigde software installeren of gewenste gegevens kopiëren en vervolgens de virtuele machine te generaliseren. Volg de stappen [Generalize van de virtuele machine](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Volg de stappen voor [aanmelden bij Azure PowerShell](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). Zie het installeren van Azure PowerShell [overzicht van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. Volg vervolgens de stappen voor [ongedaan gemaakt van de virtuele machine en de status ingesteld op algemene](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    U ziet dat de virtuele machine toewijzing ongedaan wordt gemaakt in de Azure-portal:

    ![Zorg ervoor dat de virtuele machine in de toewijzing is opgeheven](media/batch-custom-images/vm-status-deallocated.png)

6.  Maken en opslaan van de VM-installatiekopie voor het gebruik van Azure Storage de [opslaan AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) PowerShell-cmdlet. Volg de instructies die worden beschreven [maken van de installatiekopie](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    De VM-installatiekopie wordt opgeslagen in de Azure Storage-account gemaakt wanneer de virtuele machine is gemaakt, zoals u in stap 1 van deze procedure. de cmdlet opslaan AzureRmVMImage slaat de installatiekopie naar de **system** container in dit opslagaccount. De `-DestinationContainername` parameter de namen van een virtuele map in de **system** container. De `-VHDNamePrefix` parameter geeft u een voorvoegsel voor de blob-naam. Dit voorvoegsel wordt functienaam geplaatst om de blob-naam met een afbreekstreepje. 

    Stel bijvoorbeeld dat u opslaan AzureRmVMImage aanroepen met de volgende parameters:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    De afbeelding is opgeslagen in de locatie en de blob-naam hier weergegeven:

    ![Locatie van de VHD-opgeslagen in de systeemcontainer](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Een virtuele machine van Azure niet-beheerde maakt verschillende storage-accounts voor verschillende doeleinden. Als u hebt niet de naam van de opslagcontainer voor de besturingssysteemschijf notitie wanneer de virtuele machine is gemaakt, klikt u vervolgens zoeken naar de gekoppelde opslag die account bevat de **system** container. Navigeer door de **system** container vinden van de aangepaste installatiekopie met de waarden die u hebt opgegeven voor de **opslaan AzureRmVMImage** opdracht.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Een pool maken van een aangepaste installatiekopie in de portal

Als u uw aangepaste installatiekopie hebt opgeslagen en u weet dat de locatie, kunt u een Batch-pool maken vanuit die installatiekopie. Volg deze stappen voor het maken van een pool van de Azure-portal:

1. Ga in Azure Portal naar uw Batch-account. Dit account moet zich in hetzelfde abonnement en dezelfde regio als het opslagaccount met de aangepaste afbeelding. 
2. In de **instellingen** venster aan de linkerkant, selecteer de **Pools** menu-item.
3. In de **Pools** Selecteer de **toevoegen** opdracht.
4. Op de **van toepassingen toevoegen** Selecteer **aangepaste installatiekopie (Linux/Windows)** van de **afbeeldingstype** vervolgkeuzelijst. In de portal wordt de kiezer **Aangepaste installatiekopie** weergegeven. Navigeer naar het opslagaccount waarin de aangepaste installatiekopie zich bevindt en selecteer de gewenste VHD in de container. 
5. Selecteer de juiste **aanbieding-Publisher-Sku** voor uw aangepaste VHD.
6. Geef de overige instellingen, inclusief vereist de **knooppuntgrootte**, **gericht op specifieke knooppunten**, en **knooppunten prioriteit laag**, maar ook een optionele instellingen gewenst.

    Bijvoorbeeld: voor een aangepaste installatiekopie van de Microsoft Windows Server Datacenter 2016 de **van toepassingen toevoegen** venster wordt weergegeven als volgt te werk:

    ![Toepassingen toevoegen van aangepaste Windows-installatiekopie](media/batch-custom-images/add-pool-custom-image.png)
  
Als u wilt controleren of een bestaande pool is gebaseerd op een aangepaste installatiekopie, Zie de **besturingssysteem** eigenschap in de sectie voor de resource-samenvatting van de **groep** venster. Als de groep is gemaakt vanuit een aangepaste installatiekopie, is ingesteld op **aangepaste VM-installatiekopie**.

Alle aangepaste VHD's die zijn gekoppeld aan een groep worden weergegeven op de groep **eigenschappen** venster.
 
## <a name="next-steps"></a>Volgende stappen

- Zie voor een gedetailleerd overzicht van Batch [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md).
- Zie voor meer informatie over het maken van een Batch-account [een Batch-account maken met de Azure portal](batch-account-create-portal.md).