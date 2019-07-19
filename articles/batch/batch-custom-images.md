---
title: Azure Batch groep inrichten vanuit een aangepaste installatie kopie | Microsoft Docs
description: Maak een batch-pool van een aangepaste installatie kopie om reken knooppunten in te richten die de software en gegevens bevatten die u nodig hebt voor uw toepassing. Aangepaste installatie kopieën zijn een efficiënte manier om reken knooppunten te configureren om uw batch-workloads uit te voeren.
services: batch
author: laurenhughes
manager: gwallace
ms.service: batch
ms.topic: article
ms.date: 04/15/2019
ms.author: lahugh
ms.openlocfilehash: 54456ff48ca7104cc1ba10ddc47cec1bc364ddf6
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323692"
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Een aangepaste installatie kopie gebruiken om een pool van virtuele machines te maken 

Wanneer u een Azure Batch groep maakt met behulp van de configuratie van de virtuele machine, geeft u een VM-installatie kopie op die het besturings systeem levert voor elk reken knooppunt in de pool. U kunt een pool met virtuele machines maken met een ondersteunde installatie kopie van Azure Marketplace of met een aangepaste installatie kopie (een VM-installatie kopie die u zelf hebt gemaakt en geconfigureerd). De aangepaste installatie kopie moet een *beheerde installatie kopie* bron zijn in hetzelfde Azure-abonnement en dezelfde regio als het batch-account.

## <a name="benefits-of-custom-images"></a>Voor delen van aangepaste installatie kopieën

Wanneer u een aangepaste installatie kopie opgeeft, hebt u controle over de configuratie van het besturings systeem en het type besturings systeem en de gegevens schijven die moeten worden gebruikt. Uw aangepaste installatie kopie kan toepassingen en referentie gegevens bevatten die beschikbaar worden op alle knoop punten in de batch-pool zodra ze zijn ingericht.

Als u een aangepaste afbeelding gebruikt, bespaart u tijd in het voorbereiden van de reken knooppunten van uw pool om uw batch-workload uit te voeren. Hoewel u een installatie kopie van Azure Marketplace kunt gebruiken en op elk reken knooppunt software moet installeren na het inrichten, is het mogelijk efficiënter om een aangepaste installatie kopie te gebruiken.

Het gebruik van een aangepaste installatie kopie die voor uw scenario is geconfigureerd, kan verschillende voor delen bieden:

- **Het besturings systeem (OS) configureren**. U kunt de configuratie van de besturingssysteem schijf van de installatie kopie aanpassen. 
- **Installeer toepassingen vooraf.** Installeer toepassingen op de besturingssysteem schijf. Dit is efficiënter en minder fout gevoelig dan het installeren van toepassingen na het inrichten van de reken knooppunten met behulp van een begin taak.
- **Bespaar tijd voor opnieuw opstarten op Vm's.** Voor de installatie van toepassingen moet u de virtuele machine meestal opnieuw opstarten. Dit is tijdrovend. U kunt de tijd voor opnieuw opstarten besparen door toepassingen vooraf te installeren. 
- **Kopieer een zeer grote hoeveelheid gegevens eenmaal.** Statische gegevens delen van de beheerde aangepaste installatie kopie maken door deze te kopiëren naar de gegevens schijven van een beheerde installatie kopie. Dit hoeft slechts één keer te worden gedaan en er worden gegevens beschikbaar gemaakt voor elk knoop punt van de groep.
- **Keuze van schijf typen.** U hebt de keuze om Premium Storage te gebruiken voor de besturingssysteem schijf en de gegevens schijf.
- **Verg root Pools tot grote grootten.** Wanneer u een beheerde aangepaste installatie kopie gebruikt voor het maken van een pool, kan de pool groeien zonder dat u kopieën van de virtuele harde schijven van de afbeelding moet maken.

## <a name="prerequisites"></a>Vereisten

- **Een beheerde installatie kopie bron**. Als u een groep virtuele machines wilt maken met behulp van een aangepaste installatie kopie, moet u een beheerde installatie kopie bron hebben of maken in hetzelfde Azure-abonnement en dezelfde regio als het batch-account. De installatie kopie moet worden gemaakt op basis van moment opnamen van de besturingssysteem schijf van de VM en optioneel op de gekoppelde gegevens schijven. Zie de volgende sectie voor meer informatie en stappen om een beheerde installatie kopie voor te bereiden.
  - Gebruik een unieke aangepaste installatie kopie voor elke groep die u maakt.
  - Als u een groep wilt maken met de installatie kopie met behulp van de batch-Api's, geeft u de **resource-id** op van de afbeelding. Dit is van het formulier `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`. Als u de portal wilt gebruiken, gebruikt u de **naam** van de installatie kopie.  
  - De beheerde installatie kopie bron moet bestaan voor de levens duur van de groep zodat deze kan worden geschaald en kan worden verwijderd nadat de groep is verwijderd.

- **Azure Active Directory (Aad)-verificatie**. De batch-client-API moet gebruikmaken van AAD-verificatie. Azure Batch ondersteuning voor AAD wordt beschreven in [batch-service oplossingen verifiëren met Active Directory](batch-aad-auth.md).

## <a name="prepare-a-custom-image"></a>Een aangepaste installatie kopie voorbereiden

In azure kunt u een beheerde installatie kopie voorbereiden op basis van moment opnamen van het besturings systeem en de gegevens schijven van een Azure VM, van een gegeneraliseerde Azure-VM met beheerde schijven of van een gegeneraliseerde on-premises VHD die u uploadt. Als u batch-Pools betrouwbaar wilt schalen met een aangepaste installatie kopie, kunt u het beste een beheerde installatie kopie maken met *alleen* de eerste methode: met behulp van moment opnamen van de schijven van de virtuele machine. Raadpleeg de volgende stappen om een virtuele machine voor te bereiden, een moment opname te maken en een installatie kopie van de moment opname.

### <a name="prepare-a-vm"></a>Een virtuele machine voorbereiden

Als u een nieuwe VM voor de installatie kopie maakt, gebruikt u een Azure Marketplace-installatie kopie van de eerste partij die door batch wordt ondersteund als basis installatie kopie voor uw beheerde installatie kopie. Alleen installatie kopieën van de eerste partij kunnen worden gebruikt als basis installatie kopie. Voor een volledige lijst met voor beelden van Azure Marketplace-installatie kopieën die door Azure Batch worden ondersteund, raadpleegt u de [List node agent sku's](/java/api/com.microsoft.azure.batch.protocol.accounts.listnodeagentskus) -bewerking.

> [!NOTE]
> U kunt geen afbeelding van derden gebruiken die aanvullende licentie-en aankoop voorwaarden als uw basis installatie kopie heeft. Zie de richt lijnen [voor Linux](../virtual-machines/linux/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) -of [Windows](../virtual-machines/windows/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) -vm's voor meer informatie over deze installatie kopieën voor Marketplace.


* Zorg ervoor dat de virtuele machine is gemaakt met een beheerde schijf. Dit is de standaard instelling voor opslag wanneer u een VM maakt.
* Installeer geen Azure-extensies, zoals de aangepaste script extensie, op de VM. Als de installatie kopie een vooraf geïnstalleerde extensie bevat, kan Azure problemen ondervinden bij het implementeren van de batch-pool.
* Wanneer u gekoppelde gegevens schijven gebruikt, moet u de schijven koppelen en Format teren vanuit een VM om ze te gebruiken.
* Zorg ervoor dat de basis installatie kopie van het besturings systeem dat u opgeeft, gebruikmaakt van het standaard tijdelijke station. De batch-knooppunt agent verwacht momenteel het standaard tijdelijke station.
* Zodra de virtuele machine wordt uitgevoerd, maakt u verbinding met de VM via RDP (voor Windows) of SSH (voor Linux). Installeer de benodigde software of kopieer de gewenste gegevens.  

### <a name="create-a-vm-snapshot"></a>Een VM-moment opname maken

Een moment opname is een volledige, alleen-lezen kopie van een VHD. Als u een moment opname van het besturings systeem of de gegevens schijven van een virtuele machine wilt maken, kunt u de Azure Portal of opdracht regel Programma's gebruiken. Zie de richt lijnen voor [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) -of [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md) -vm's voor een overzicht van de stappen en opties voor het maken van een moment opname.

### <a name="create-an-image-from-one-or-more-snapshots"></a>Een installatie kopie maken van een of meer moment opnamen

Als u een beheerde installatie kopie wilt maken op basis van een moment opname, gebruikt u de opdracht regel Programma's van Azure, zoals de opdracht [AZ image Create](/cli/azure/image) . U kunt een installatie kopie maken door een moment opname van de besturingssysteem schijf en eventueel een of meer moment opnamen van de gegevens schijf op te geven.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Een groep maken op basis van een aangepaste installatie kopie in de portal

Nadat u uw aangepaste installatie kopie hebt opgeslagen en u de resource-ID of-naam weet, maakt u een batch-pool van die afbeelding. De volgende stappen laten zien hoe u een groep maakt op basis van de Azure Portal.

> [!NOTE]
> Als u de pool maakt met behulp van een van de batch-Api's, moet u ervoor zorgen dat de identiteit die u voor AAD-verificatie gebruikt, machtigingen heeft voor de afbeeldings bron. Zie [batch-service oplossingen verifiëren met Active Directory](batch-aad-auth.md).
>
> De resource voor de beheerde installatie kopie moet bestaan voor de levens duur van de groep. Als de onderliggende resource wordt verwijderd, kan de groep niet worden geschaald. 

1. Ga in Azure Portal naar uw Batch-account. Dit account moet zich in hetzelfde abonnement en dezelfde regio bevinden als de resource groep met de aangepaste installatie kopie. 
2. Selecteer in het venster **instellingen** aan de linkerkant de menu opdracht **groepen** .
3. Selecteer in het venster **groepen** de opdracht **toevoegen** .
4. Selecteer in het venster **groep toevoegen** de optie **aangepaste installatie kopie (Linux/Windows)** in de vervolg keuzelijst **afbeeldings type** . Selecteer in de vervolg keuzelijst **aangepaste VM-installatie** kopie de naam van de installatie kopie (korte vorm van de resource-id).
5. Selecteer de juiste **Uitgever/aanbieding/SKU** voor uw aangepaste installatie kopie.
6. Geef de overige vereiste instellingen op, zoals de **knooppunt grootte**, het **doel toegewezen knoop punten**en **knoop punten met lage prioriteit**, evenals de gewenste optionele instellingen.

    Bijvoorbeeld, voor een aangepaste installatie kopie van micro soft Windows Server Data Center 2016, wordt het venster **groep toevoegen** weer gegeven, zoals hieronder wordt weer gegeven:

    ![Groep toevoegen vanuit aangepaste Windows-installatie kopie](media/batch-custom-images/add-pool-custom-image.png)
  
Als u wilt controleren of een bestaande groep is gebaseerd op een aangepaste installatie kopie, raadpleegt u de eigenschap van het **besturings systeem** in de sectie Resource Summary van het venster **groep** . Als de groep is gemaakt op basis van een aangepaste installatie kopie, wordt deze ingesteld op een **aangepaste VM-installatie kopie**.

Alle aangepaste installatie kopieën die aan een groep zijn gekoppeld, worden weer gegeven in het **Eigenschappen** venster van de groep.

## <a name="considerations-for-large-pools"></a>Overwegingen voor grote Pools

Als u van plan bent een pool met honderden Vm's of meer te maken met behulp van een aangepaste installatie kopie, is het belang rijk dat u de voor gaande richt lijnen volgt voor het gebruik van een installatie kopie die is gemaakt van een VM-moment opname.

Let ook op het volgende:

- **Grootte limieten** : batch beperkt de pool grootte tot 2500 toegewezen reken knooppunten of de knoop punten met een lage prioriteit van 1000 wanneer u een aangepaste installatie kopie gebruikt.

  Als u dezelfde installatie kopie (of meerdere installatie kopieën op basis van dezelfde onderliggende moment opname) gebruikt om meerdere groepen te maken, kunnen de totale reken knooppunten in de groepen niet groter zijn dan de voor gaande limieten. Het is niet raadzaam een afbeelding of de onderliggende moment opname te gebruiken voor meer dan één groep.

  Limieten kunnen worden verminderd als u de groep configureert met [binnenkomende NAT-Pools](pool-endpoint-configuration.md).

- **Grootte van de time-out wijzigen** : als uw pool een vast aantal knoop punten bevat (niet automatisch schalen), verhoogt u de eigenschap resizeTimeout van de groep naar een waarde van 20-30 minuten. Als uw pool niet binnen de time-outperiode de doel grootte bereikt, moet u een andere grootte voor het [formaat van de bewerking](/rest/api/batchservice/pool/resize)uitvoeren.

  Als u een pool plant met meer dan 300 reken knooppunten, moet u mogelijk meerdere malen het formaat van de groep wijzigen om de doel grootte te bereiken.

## <a name="considerations-for-using-packer"></a>Overwegingen voor het gebruik van Packer

Het maken van een beheerde installatie kopie bron rechtstreeks met de Packer kan alleen worden uitgevoerd met de modus voor de gebruikers abonnement-batch accounts. Voor accounts van de batch-service modus moet u eerst een VHD maken en vervolgens de VHD importeren in een beheerde installatie kopie bron. Afhankelijk van de pool toewijzings modus (gebruikers abonnement of batch-service), kunnen de stappen voor het maken van een beheerde afbeeldings resource verschillen.

Zorg ervoor dat de resource die wordt gebruikt om de beheerde installatie kopie te maken, bestaat voor de levens duur van elke groep die verwijst naar de aangepaste installatie kopie. Als u dit niet doet, kan dit leiden tot geheugen toewijzings fouten en/of het formaat van fouten. 

Als de afbeelding of de onderliggende resource wordt verwijderd, wordt er mogelijk een fout bericht weer gegeven `There was an error encountered while performing the last resize on the pool. Please try resizing the pool again. Code: AllocationFailed`die vergelijkbaar is met:. Als dit het geval is, moet u ervoor zorgen dat de onderliggende resource niet is verwijderd.

Zie [een Linux-installatie kopie bouwen met](../virtual-machines/linux/build-image-with-packer.md) een Packer of [een Windows-installatie kopie bouwen met Packer](../virtual-machines/windows/build-image-with-packer.md)voor meer informatie over het gebruik van Packer voor het maken van een virtuele machine.

## <a name="next-steps"></a>Volgende stappen

- Zie [grootschalige parallelle reken oplossingen ontwikkelen met batch](batch-api-basics.md)voor een uitgebreid overzicht van batch.
