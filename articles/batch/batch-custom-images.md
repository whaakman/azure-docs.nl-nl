---
title: Azure Batch-pool van een aangepaste installatiekopie inrichten | Microsoft Docs
description: Maak een Batch pool van een aangepaste installatiekopie voor het inrichten van rekenknooppunten die de software en gegevens die u nodig hebt voor uw toepassing bevatten. Aangepaste installatiekopieën zijn een efficiënte manier voor het configureren van rekenknooppunten voor het uitvoeren van uw Batch-workloads.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 10/04/2018
ms.author: danlep
ms.openlocfilehash: 7d0526dd233afd3976b22d257300681db0bfcead
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885203"
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Een aangepaste installatiekopie gebruiken om een pool van virtuele machines te maken 

Wanneer u een Azure Batch-pool met behulp van de configuratie van de virtuele Machine maakt, geeft u een VM-installatiekopie met het besturingssysteem voor elk knooppunt in de groep. U kunt een pool van virtuele machines maken met een ondersteunde Azure Marketplace-installatiekopie, of met een aangepaste installatiekopie (een VM-installatiekopie u hebt gemaakt en geconfigureerd zelf). De aangepaste afbeelding moet een *beheerde installatiekopie* resource in hetzelfde Azure-abonnement en regio als het Batch-account.

## <a name="why-use-a-custom-image"></a>Waarom een aangepaste installatiekopie gebruiken?

Wanneer u een aangepaste installatiekopie opgeeft, hebt u controle over de configuratie van het besturingssysteem en het type besturingssysteem en gegevensschijven moeten worden gebruikt. Uw aangepaste installatiekopie kunt opnemen, toepassingen en gegevens die beschikbaar op alle knooppunten van de Batch pool zodra deze zijn ingericht.

Met behulp van een aangepaste installatiekopie, bespaart u tijd bij de voorbereiding van rekenknooppunten om uit te voeren van uw Batch-workload van uw pool. Tijdens het gebruik van een Azure Marketplace-installatiekopie en software installeren op elk knooppunt na het inrichten, is met een aangepaste installatiekopie mogelijk efficiënter.

Met behulp van een aangepaste installatiekopie die is geconfigureerd voor uw scenario kan bieden verschillende voordelen:

- **Configureren van het besturingssysteem (OS)**. U kunt de configuratie van de besturingssysteemschijf van de installatiekopie kunt aanpassen. 
- **Toepassingen vooraf worden geïnstalleerd.** Voorafgaand aan installatie toepassingen op de besturingssysteemschijf, dit is efficiënter en minder gevoelig voor fouten dan het installeren van toepassingen na het inrichten van de compute-knooppunten met behulp van een begintaak.
- **Opnieuw opstarten tijd besparen op virtuele machines.** De installatie van toepassing is doorgaans nodig voor het opnieuw opstarten van de virtuele machine, die is tijdrovend. U kunt opnieuw opstarten tijd besparen door vooraf installeren van toepassingen. 
- **Zeer grote hoeveelheden gegevens eenmaal kopiëren.** Statische gegevensonderdeel van de beheerde aangepaste installatiekopie maken door deze te kopiëren naar een beheerde installatiekopie gegevensschijven. Dit alleen moet één keer worden uitgevoerd en maakt gegevens beschikbaar zijn voor elk knooppunt van de groep.
- **Keuze voor schijftypen.** U hebt de keuze van het gebruik van premium-opslag voor de besturingssysteemschijf en de gegevensschijf.
- **Breid pools te grote grootten.** Wanneer u een beheerde aangepaste installatiekopie gebruiken om een pool te maken, worden de toepassingen kan meegroeien zonder dat u kopieën van blob VHD's van de installatiekopie te maken. 


## <a name="prerequisites"></a>Vereisten

- **De resource van een beheerde installatiekopie**. Als u wilt een pool van virtuele machines met een aangepaste installatiekopie maakt, moet u zijn of een resource beheerde installatiekopie maken in hetzelfde Azure-abonnement en regio als het Batch-account. De afbeelding moet worden gemaakt van momentopnamen van de besturingssysteemschijf van de virtuele machine en (optioneel) de gekoppelde gegevensschijven. Zie de volgende sectie voor meer informatie en stappen voor het voorbereiden van een beheerde installatiekopie. 
  - Een unieke aangepaste installatiekopie gebruiken voor elke groep die u maakt.
  - Als u wilt een pool maken met de installatiekopie met behulp van de Batch-API's, geef de **resource-ID** van de installatiekopie, die van het formulier `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`. Voor het gebruik van de portal, gebruikt u de **naam** van de installatiekopie.  
  - De resource beheerde installatiekopie moet bestaan voor de levensduur van de pool omhoog zodat en kan worden verwijderd nadat de groep is verwijderd.

- **Verificatie van Azure Active Directory (AAD)**. De Batch-client-API moet de AAD-verificatie gebruiken. Azure Batch-ondersteuning voor AAD wordt gedocumenteerd in [verifiëren Batch-service-oplossingen met Active Directory](batch-aad-auth.md).

## <a name="prepare-a-custom-image"></a>Een aangepaste installatiekopie voorbereiden

U kunt een beheerde installatiekopie van momentopnamen van een Azure-VM-besturingssysteem en gegevensschijven, van een gegeneraliseerde Azure-VM met beheerde schijven of vanaf een gegeneraliseerde on-premises VHD die u uploadt voorbereiden in Azure. Als u wilt schalen Batch-pools op betrouwbare wijze met een aangepaste installatiekopie, wordt aangeraden het maken van een beheerde installatiekopie met *alleen* de eerste methode: met momentopnamen van de VM schijven. Zie de volgende stappen voor het voorbereiden van een virtuele machine, maakt u een momentopname en een installatiekopie maken van de momentopname. 

### <a name="prepare-a-vm"></a>Een virtuele machine voorbereiden 

Als u een nieuwe virtuele machine voor de installatiekopie maakt, gebruikt u de installatiekopie van een Azure Marketplace ondersteund door de Batch als de basisinstallatiekopie voor uw beheerde installatiekopie en vervolgens aan te passen.  Als u een lijst met verwijzingen naar afbeeldingen Azure Marketplace wordt ondersteund door Azure Batch, raadpleegt u de [knooppuntagent-SKU's lijst](/rest/api/batchservice/account/listnodeagentskus) bewerking. U kunt een installatiekopie van derden niet gebruiken als uw basisinstallatiekopie.

* Zorg ervoor dat de virtuele machine wordt gemaakt met een beheerde schijf. Dit is de standaardinstelling voor de opslag bij het maken van een virtuele machine.
* Moet de Azure-extensies, zoals de Custom Script-extensie niet installeren op de virtuele machine. Als de installatiekopie een vooraf geïnstalleerde extensie bevat, kan Azure problemen optreden bij het implementeren van de Batch-pool.
* Zorg ervoor dat de OS-basisinstallatiekopie u het reguliere tijdelijke station gebruikt. De Batch-knooppuntagent verwacht momenteel het reguliere tijdelijke station.
* Zodra de virtuele machine wordt uitgevoerd, verbinding te maken via RDP (voor Windows) of SSH (voor Linux). Alle benodigde software installeren of de gewenste gegevens te kopiëren.  

### <a name="create-a-vm-snapshot"></a>Een VM-momentopname maken

Een momentopname is een volledige, alleen-lezen kopie van een VHD. U kunt de Azure portal of de opdrachtregelprogramma's gebruiken voor het maken van een momentopname van het besturingssysteem van een virtuele machine of gegevensschijven. Voor stappen en opties voor het maken van een momentopname, Zie de richtlijnen voor [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) of [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md) VM's.

### <a name="create-an-image-from-one-or-more-snapshots"></a>Een installatiekopie maken van een of meer momentopnamen

Gebruik voor het maken van een beheerde installatiekopie van een momentopname van een Azure-opdrachtregelprogramma's zoals de [az afbeelding maken](/cli/azure/image#az_image_create) opdracht. U kunt een installatiekopie maken door de momentopname van een OS-schijf en optioneel een of meer momentopnamen van gegevens de schijf op te geven.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Een groep maken van een aangepaste installatiekopie in de portal

Wanneer u uw aangepaste installatiekopie hebt opgeslagen en u weet dat de resource-ID of naam, maakt u een Batch-pool van die installatiekopie. De volgende stappen laten zien hoe u een groep maken vanuit Azure portal.

> [!NOTE]
> Als u de groep met een van de Batch-API's maakt, zorg ervoor dat de identiteit die voor AAD-verificatie u machtigingen voor de bron van de installatiekopie heeft. Zie [verifiëren Batch-service-oplossingen met Active Directory](batch-aad-auth.md).
>

1. Ga in Azure Portal naar uw Batch-account. Dit account moet zich in hetzelfde abonnement en dezelfde regio als de resourcegroep die de aangepaste installatiekopie bevat. 
2. In de **instellingen** venster aan de linkerkant, selecteer de **Pools** menu-item.
3. In de **Pools** venster de **toevoegen** opdracht.
4. Op de **groep toevoegen** venster **aangepaste installatiekopie (Linux/Windows)** uit de **afbeeldingstype** vervolgkeuzelijst. Uit de **aangepaste VM-installatiekopie** vervolgkeuzelijst, selecteer de naam van de installatiekopie (verkorte vorm van de resource-ID).
5. Selecteer de juiste **uitgever/aanbieding/Sku** voor uw aangepaste installatiekopie.
6. Geef de overige vereiste instellingen, met inbegrip van de **knooppuntgrootte**, **doel toegewezen knooppunten**, en **lage prioriteit knooppunten**, evenals een optionele instellingen desgewenst.

    Bijvoorbeeld: voor een aangepaste installatiekopie van de Microsoft Windows Server Datacenter 2016 de **groep toevoegen** venster wordt weergegeven zoals hieronder wordt weergegeven:

    ![Groep toevoegen vanuit een aangepaste installatiekopie van Windows](media/batch-custom-images/add-pool-custom-image.png)
  
Als u wilt controleren of een bestaande pool is gebaseerd op een aangepaste installatiekopie, Zie de **besturingssysteem** eigenschap in de resource-samenvatting van de **Pool** venster. Als de groep is gemaakt van een aangepaste installatiekopie, deze is ingesteld op **aangepaste VM-installatiekopie**.

Alle aangepaste installatiekopieën die zijn gekoppeld aan een groep worden weergegeven op van de pool **eigenschappen** venster.

## <a name="considerations-for-large-pools"></a>Overwegingen voor grote groepen

Als u van plan bent om te maken van een pool met honderden virtuele machines of met behulp van een aangepaste installatiekopie, is het belangrijk dat u het voorafgaande advies voor het gebruik van een installatiekopie gemaakt op basis van een momentopname van een virtuele machine.

Let ook op het volgende:

- **Limieten voor grootte** -Batch beperkt de grootte van de groep 2500 toegewezen rekenknooppunten of 1000 knooppunten met lage prioriteit, wanneer u een aangepaste installatiekopie gebruiken.

  Als u dezelfde installatiekopie (of meerdere installatiekopieën op basis van de dezelfde onderliggende momentopname) gebruiken voor het maken van meerdere groepen, de totale compute-knooppunten in de groepen niet langer zijn dan de voorgaande limieten. We raden gebruik van een afbeelding of de momentopname van de onderliggende voor meer dan één groep.

  Limieten kunnen worden verlaagd, indien configuratie van de pool met [inkomende NAT-pools](pool-endpoint-configuration.md).

- **Time-out voor formaat wijzigen** : als uw groep een vast bevat aantal knooppunten (niet voor automatisch schalen), verhoogt u de eigenschap resizeTimeout van de groep op een waarde zoals 20-30 minuten. Als de pool niet van de doelgrootte binnen de time-outperiode bereikt, voert u een andere [bewerking formaat](/rest/api/batchservice/pool/resize).

  Als u van plan een pool met meer dan 300 rekenknooppunten bent, moet u mogelijk de groep meerdere keren vergroten of verkleinen om te bereiken, de doelgrootte.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een gedetailleerd overzicht van Batch, [grootschalige parallelle rekenoplossingen ontwikkelen met Batch](batch-api-basics.md).
