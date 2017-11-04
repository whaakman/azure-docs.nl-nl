---
title: "Azure Batch-pools van aangepaste installatiekopieën inrichten | Microsoft Docs"
description: "U kunt een Batch toepassingen van een aangepaste installatiekopie die u wilt inrichten rekenknooppunten die de software en gegevens die u nodig hebt voor uw toepassing bevatten. Aangepaste installatiekopieën zijn een efficiënte manier voor het configureren van rekenknooppunten om uit te voeren van uw Batch-workloads."
services: batch
author: v-dotren
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 10/11/2017
ms.author: v-dotren
ms.openlocfilehash: d62abd673f89fd51edba721119d1680762a60c76
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="use-a-managed-custom-image-to-create-a-pool-of-virtual-machines"></a>Een beheerde aangepaste installatiekopie gebruiken voor een pool van virtuele machines maken 

Wanneer u een Azure Batch-pool met behulp van de configuratie van de virtuele Machine maakt, geeft u een VM-installatiekopie die het besturingssysteem voor elk rekenknooppunt in de groep biedt. U kunt een pool van virtuele machines maken met een Azure Marketplace-installatiekopie of met een aangepaste installatiekopie (een VM-installatiekopie u hebt gemaakt en geconfigureerd zelf). De aangepaste afbeelding moet een *begeleide afbeelding* bron in het hetzelfde Azure-abonnement en dezelfde regio als het Batch-account.

## <a name="why-use-a-custom-image"></a>Waarom zou ik een aangepaste installatiekopie gebruiken?
Wanneer u een aangepaste installatiekopie opgeeft, hebt u controle over de configuratie van het besturingssysteem en het type besturingssysteem en gegevensschijven moet worden gebruikt. Uw aangepaste installatiekopie kunt opnemen, toepassingen en verwijzen naar gegevens die beschikbaar zijn op alle knooppunten van de Batch pool komen zodra deze zijn ingericht.

Met een aangepaste installatiekopie, bespaart u tijd bij het voorbereiden van uw pool van rekenknooppunten om uit te voeren van uw Batch-werkbelasting. Terwijl u kunt een Azure Marketplace-installatiekopie gebruiken en software op elk rekenknooppunt na het inrichten installeren, met een aangepaste installatiekopie mogelijk efficiënter zijn.

Met een aangepaste installatiekopie die is geconfigureerd voor uw scenario kan verschillende voordelen bieden:

- **Configureren van het besturingssysteem (OS)**. U kunt speciale configuratie van het besturingssysteem op de schijf van de aangepaste installatiekopie besturingssysteem uitvoeren. 
- **Toepassingen vooraf te installeren.** Kunt u een aangepaste installatiekopie met vooraf geïnstalleerde toepassingen op de schijf OS, efficiënter minder en gevoelig voor fouten dan het installeren van toepassingen na het inrichten van de rekenknooppunten StartTask gebruiken.
- **Opnieuw opstarten tijd besparen op virtuele machines.** De installatie van toepassing is doorgaans opnieuw wordt opgestart nadat de virtuele machine die is tijdrovend. U kunt opnieuw opstarten tijd besparen vooraf door toepassingen te installeren. 
- **Zeer grote hoeveelheden gegevens eenmaal kopiëren.** U kunt statische gegevensonderdeel van de beheerde aangepaste afbeelding maken door kopiëren naar een begeleide afbeelding gegevensschijven. Dit alleen moet één keer worden uitgevoerd en zijn de gegevens beschikbaar zijn voor elk knooppunt van de groep.
- **De keuze van schijftypen.** U kunt een beheerde aangepaste installatiekopie maken vanaf een VHD, vanaf een beheerde schijf van een virtuele machine van Azure een momentopname van deze schijven of uw eigen Linux of Windows-installatie die u hebt geconfigureerd. U hebt de keuze van het gebruik van premium-opslag voor de besturingssysteemschijf en de gegevensschijf.
- **Groepen worden uitgebreid naar elke grootte.** Wanneer u een beheerde aangepaste installatiekopie gebruikt om een pool te maken, kan de pool worden uitgebreid naar elke grootte die u aanvraagt. U hoeft niet kopieën van de installatiekopie blob VHD's voor het aantal virtuele machines te maken. 


## <a name="prerequisites"></a>Vereisten

- **Een beheerde Afbeeldingsbron**. Voor het maken van een pool van virtuele machines met een aangepaste installatiekopie, moet u een begeleide afbeelding-resource maken in de dezelfde Azure-abonnement en dezelfde regio als het Batch-account. Zie de volgende sectie voor de opties voor het voorbereiden van een begeleide afbeelding.
- **Verificatie met Azure Active Directory (AAD)**. De client-API van de Batch moet AAD-verificatie gebruiken. Azure Batch-ondersteuning voor AAD wordt beschreven in [oplossingen van de service Batch verifiëren met Active Directory](batch-aad-auth.md).

    
## <a name="prepare-a-custom-image"></a>Een aangepaste installatiekopie voorbereiden
U kunt een begeleide afbeelding voorbereiden vanaf een VHD, een Azure-VM met beheerde schijven of een VM-momentopname. 

Bij het voorbereiden van uw installatiekopie, houd er rekening mee houden de volgende punten:

* Zorg ervoor dat de basisinstallatiekopie voor het besturingssysteem die u gebruikt om uw Batch-pools in te richten geen vooraf geïnstalleerde Azure-extensies bevat, zoals de extensie Aangepast script. Als de installatiekopie een vooraf geïnstalleerde extensie bevat, kunnen er binnen Azure problemen optreden bij het implementeren van de VM.
* Zorg ervoor dat de basis OS-installatiekopie die u opgeeft het standaardstation van tijdelijke gebruikt. De agent Batch-knooppunt wordt momenteel het standaardstation van tijdelijke verwacht.
* De begeleide afbeelding resource waarnaar wordt verwezen door een Batch-Pool kan niet worden verwijderd voor de levensduur van de groep. Als de begeleide afbeelding resource wordt verwijderd, klikt u vervolgens de groep kan niet worden uitgebreid een verdere. 

### <a name="to-create-a-managed-image"></a>Een begeleide afbeelding maken
Eventuele bestaande voorbereide Windows of Linux-besturingssysteemschijf kunt u een begeleide afbeelding maken. Bijvoorbeeld, als u een lokale installatiekopie gebruiken wilt, vervolgens uploaden de lokale schijf op een Azure Storage-account dat zich in hetzelfde abonnement en dezelfde regio als uw Batch-account met behulp van AzCopy of een ander hulpprogramma voor uploaden. Zie voor gedetailleerde stappen voor het uploaden van een VHD en maken van een begeleide afbeelding de richtlijn voor [Windows](../virtual-machines/windows/upload-generalized-managed.md) of [Linux](../virtual-machines/linux/upload-vhd.md) virtuele machines.

U kunt ook een begeleide afbeelding voorbereiden van een nieuwe of bestaande virtuele machine in Azure of een VM-momentopname. 

* Als u een nieuwe virtuele machine maakt, kunt u een Azure Marketplace-installatiekopie gebruiken als de basisinstallatiekopie voor uw beheerde installatiekopie en deze aanpassen. 

* Als u van plan bent om vast te leggen van de installatiekopie via de portal, moet u ervoor dat de virtuele machine is gemaakt met een beheerde schijf. Dit is de standaardinstelling voor de opslag bij het maken van een virtuele machine.

* Zodra de virtuele machine wordt uitgevoerd, verbinding maken via RDP (voor Windows) of SSH (voor Linux). Alle benodigde software installeren of gewenste gegevens kopiëren en vervolgens de virtuele machine te generaliseren.  

Zie voor stappen generalize van een virtuele machine van Azure en een begeleide afbeelding maken voor de richtlijnen voor [Windows](../virtual-machines/windows/capture-image-resource.md) of [Linux](../virtual-machines/linux/capture-image.md) virtuele machines.

Afhankelijk van hoe u van plan bent een Batch-pool maken met de installatiekopie, moet u de volgende id voor de installatiekopie:

* Als u wilt een pool maken met de installatiekopie met de Batch-API's de **resource-ID** van de installatiekopie van het formulier `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`. 
* Als u van plan bent gebruik van de portal de **naam** van de afbeelding. 





## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Een pool maken van een aangepaste installatiekopie in de portal

Als u uw aangepaste installatiekopie hebt opgeslagen en u de resource-ID of naam weet, kunt u een Batch-pool maken vanuit die installatiekopie. De volgende stappen ziet u een groep maken vanuit de Azure-portal.

> [!NOTE]
> Als u de groep met een van de Batch-API's maakt, zorg ervoor dat de identiteit die voor AAD-verificaties u machtigingen voor de bron van de installatiekopie heeft. Zie [oplossingen van de service Batch verifiëren met Active Directory](batch-aad-auth.md).
>

1. Ga in Azure Portal naar uw Batch-account. Dit account moet zich in hetzelfde abonnement en dezelfde regio als de resourcegroep met de aangepaste afbeelding. 
2. In de **instellingen** venster aan de linkerkant, selecteer de **Pools** menu-item.
3. In de **Pools** Selecteer de **toevoegen** opdracht.
4. Op de **van toepassingen toevoegen** Selecteer **aangepaste installatiekopie (Linux/Windows)** van de **afbeeldingstype** vervolgkeuzelijst. Van de **aangepaste VM-installatiekopie** vervolgkeuzelijst, selecteer de naam van de installatiekopie (korte versie van de resource-ID).
5. Selecteer de juiste **aanbieding-Publisher-Sku** voor uw aangepaste installatiekopie.
6. Geef de overige instellingen, inclusief vereist de **knooppuntgrootte**, **gericht op specifieke knooppunten**, en **knooppunten prioriteit laag**, maar ook een optionele instellingen gewenst.

    Bijvoorbeeld: voor een aangepaste installatiekopie van de Microsoft Windows Server Datacenter 2016 de **van toepassingen toevoegen** venster wordt weergegeven zoals hieronder wordt weergegeven:

    ![Toepassingen toevoegen van aangepaste Windows-installatiekopie](media/batch-custom-images/add-pool-custom-image.png)
  
Als u wilt controleren of een bestaande pool is gebaseerd op een aangepaste installatiekopie, Zie de **besturingssysteem** eigenschap in de sectie voor de resource-samenvatting van de **groep** venster. Als de groep is gemaakt vanuit een aangepaste installatiekopie, is ingesteld op **aangepaste VM-installatiekopie**.

Alle aangepaste installatiekopieën die zijn gekoppeld aan een groep worden weergegeven op de groep **eigenschappen** venster.
 
## <a name="next-steps"></a>Volgende stappen

- Zie voor een gedetailleerd overzicht van Batch [ontwikkelen grootschalige parallelle compute-oplossingen met Batch](batch-api-basics.md).
