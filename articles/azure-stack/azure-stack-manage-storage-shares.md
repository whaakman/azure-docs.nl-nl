---
title: Beheren van de opslagcapaciteit in Azure-Stack | Microsoft Docs
description: Bewaken en beheren van de beschikbare opslagruimte voor Azure-Stack.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: b0e694e4-3575-424c-afda-7d48c2025a62
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: dce4252846732ca5161018103438df1f9ff6146d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Opslagcapaciteit voor Azure-Stack beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

De informatie in dit artikel helpt de Azure-Stack cloud operator monitor en beheren van de opslagcapaciteit van de Azure-Stack-implementatie. De infrastructuur van de Azure-Stack wijst een subset van de totale capaciteit van de implementatie van het Azure-Stack moet worden gebruikt voor **opslagservices**. De opslagservices opslaan van gegevens van een tenant in shares op volumes die met de knooppunten van de implementatie overeenkomen.

Als een cloud-operator hebt u een beperkte hoeveelheid opslag werken met. De hoeveelheid opslagruimte wordt gedefinieerd door de oplossing die te implementeren. Uw oplossing wordt verstrekt door de leverancier van uw OEM wanneer u een oplossing met meerdere knooppunten gebruikt of door de hardware waarop u de Azure-Stack Development Kit installeren.

Omdat Azure Stack biedt geen ondersteuning voor uitbreiding van de opslagcapaciteit, is het belangrijk dat u [monitor](#monitor-shares) de beschikbare opslag om ervoor te zorgen efficiënt bewerkingen worden bewaard.  

Wanneer de resterende beschikbare capaciteit van een share beperkt wordt, wilt [ruimte beheren](#manage-available-space) om te voorkomen dat de shares capaciteit die wordt uitgevoerd.

Opties voor het beheren van capaciteit zijn onder andere:
- Capaciteit vrijmaken
- Migreren van een container

Wanneer een share is 100% gebruikt, de opslag-service is niet meer functies voor deze share. Als hulp bij het terugzetten van bewerkingen voor de share, contact op met Microsoft ondersteuning.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Volumes en shares, containers en schijven begrijpen
### <a name="volumes-and-shares"></a>Volumes en shares
De *opslagservice* partities van de beschikbare opslag in afzonderlijke en gelijkwaardige volumes die zijn toegewezen voor het opslaan van gegevens van de tenant. Het aantal volumes is gelijk aan het aantal knooppunten in de Azure-Stack-implementatie:

- Er zijn vier volumes op een implementatie met vier knooppunten. Elk volume heeft een afzonderlijke share. Het aantal shares is niet op een implementatie met meerdere knooppunten verminderd als een knooppunt verwijderd of functioneert niet goed is.
- Als u de Azure-Stack Developer Kit gebruikt, is er één volume met één share.

Omdat de service storage shares voor exclusief gebruik van storage-services, moet u niet rechtstreeks wijzigen, toevoegen of de bestanden op de shares verwijderen. Storage-services moeten op de bestanden opgeslagen in deze volumes werken.

Shares op volumes bevatten tenant gegevens. Gegevens van de tenant bevat pagina-blobs, blok-blobs, toevoeg-blobs, tabellen, wachtrijen, databases en gerelateerde metagegevens stores. Omdat de opslagobjecten (blobs, enzovoort) afzonderlijk binnen één share deel uitmaken, kan de maximale grootte van elk object kan niet groter zijn dan de grootte van een share. De maximale grootte van nieuwe objecten zijn afhankelijk van de capaciteit die op een share als niet-gebruikte ruimte overblijft wanneer dat nieuwe object wordt gemaakt.

Wanneer een share nog weinig vrije ruimte en de acties voor [vrijmaken](#reclaim-capacity) ruimte niet zijn geslaagd of niet beschikbaar, de Azure-Stack cloud-operator kunt [migreren](#migrate-a-container-between) de blob-containers van een share naar een andere.

- Zie voor meer informatie over de containers en blobs [Blob storage](azure-stack-key-features.md#blob-storage) in sleutel-functies en -concepten in Azure-Stack.
- Zie voor meer informatie over hoe tenant gebruikers met blob storage in Azure-Stack werken [Azure Stack Storage-services](/azure/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Containers
Gebruikers van de tenant maken containers die vervolgens worden gebruikt voor het opslaan van blob-gegevens. Terwijl de gebruiker in welke container besluit worden geplaatst, blobs, de storage-service een algoritme gebruikt om te bepalen op welke volume aan de container te plaatsen. Het algoritme kiest normaal gesproken het volume met de meeste beschikbare ruimte.  

Nadat een blob wordt geplaatst in een container, kan er meer ruimte nodig blob groeien. Als u nieuwe blobs en bestaande toevoegen toenemen blobs, de beschikbare ruimte op het volume van de container waarin wordt verkleind.  

Containers zijn niet beperkt tot één share. Wanneer de gecombineerde blob-gegevens in een container gebruik 80% of meer van de beschikbare ruimte groeit, de container ingevoerd *overloop* modus. In de modus overloop, worden alle nieuwe blobs die zijn gemaakt in de container toegewezen aan een ander volume met voldoende ruimte. Na verloop van tijd kan een container in de modus overloop blobs die zijn verdeeld over meerdere volumes hebben.

Wanneer 80% en 90% van de beschikbare ruimte op een volume wordt gebruikt, worden waarschuwingen in de Azure-Stack-beheerdersportal gegeven in het systeem. Cloudoperators Controleer beschikbare opslagcapaciteit, en plan de inhoud opnieuw verdelen. De storage-service niet meer functioneert als een schijf 100 is % gebruikt en er zijn geen extra waarschuwingen worden gegenereerd.

### <a name="disks"></a>Disks
VM-schijven worden toegevoegd aan containers door tenants en de schijf van een besturingssysteem bevatten. Virtuele machines kunnen ook een of meer gegevensschijven hebben. Beide typen schijven worden opgeslagen als pagina-blobs. De richtlijnen voor tenants is elke schijf in een afzonderlijke container voor het verbeteren van de prestaties van de virtuele machine te plaatsen.
- Elke container met een schijf (pagina-blob) van een virtuele machine wordt beschouwd als een gekoppelde container met de virtuele machine die eigenaar is van de schijf.
- Een container waarin elke schijf van een virtuele machine niet bijhoudt wordt beschouwd als een gratis container.

De opties voor het vrijmaken van ruimte op een gekoppelde container [zijn beperkt](#move-vm-disks).
> [!TIP]  
> Cloudoperators beheren schijven die zijn gekoppeld aan virtuele machines (VM's) die tenants wilt aan een container toevoegen mogelijk niet rechtstreeks. Bij het plannen van ruimte op de storage-shares kunt beheren, kan het echter zijn gebruikt om te begrijpen hoe schijven hebben betrekking op containers en -shares.

## <a name="monitor-shares"></a>Monitor shares
Gebruik PowerShell of het beheerportal voor het bewaken van shares, zodat u inzicht in wanneer de vrije ruimte is beperkt. Wanneer u de portal gebruikt, krijgt u waarschuwingen over shares die weinig schijfruimte.    

### <a name="use-powershell"></a>PowerShell gebruiken
Als een cloud-operator, kunt u de capaciteit van een share met de PowerShell controleren **Get-AzsStorageShare** cmdlet. De cmdlet Get-AzsStorageShare retourneert de totale toegewezen en vrije ruimte in bytes op elk van de shares.   
![Voorbeeld: Vrije ruimte voor shares retourneren](media/azure-stack-manage-storage-shares/free-space.png)

- **Totale capaciteit** is de totale ruimte in bytes die beschikbaar op de share zijn. Deze ruimte wordt gebruikt voor gegevens en metagegevens die wordt beheerd door de storage-services.
- **Capaciteit gebruikt** is de hoeveelheid gegevens in bytes dat wordt gebruikt door alle gebieden van de bestanden waarin de gegevens van de tenant en de gekoppelde metagegevens worden opgeslagen.

### <a name="use-the-administrator-portal"></a>Gebruik de beheerdersportal
Als een cloud-operator, kunt u de beheerportal om de capaciteit van alle shares weer te geven. **Ga naar opslag** > **bestandsshares** openen van de lijst met de bestandsshare waar u de gebruiksgegevens kunt bekijken.
![Voorbeeld: Opslagbestandsshares](media/azure-stack-manage-storage-shares/storage-file-shares.png)
- **Totaal aantal** is de totale ruimte in bytes die beschikbaar op de share zijn. Deze ruimte wordt gebruikt voor gegevens en metagegevens die wordt beheerd door de storage-services.
- **GEBRUIKT** is de hoeveelheid gegevens in bytes dat wordt gebruikt door alle gebieden van de bestanden waarin de gegevens van de tenant en de gekoppelde metagegevens worden opgeslagen.

### <a name="storage-space-alerts"></a>Opslag ruimte waarschuwingen
Wanneer u het beheerportal gebruikt, ontvangt u meldingen over shares die weinig schijfruimte.

> [!IMPORTANT]
> Als een cloud-operator, houd shares bereikt volledige informatie over het gebruik. Wanneer een share is 100% gebruikt, de opslag-service is niet meer functies voor deze share. Als u wilt herstellen vrije ruimte en herstelbewerkingen op een share die 100% worden gebruikt, moet u contact op met Microsoft ondersteuning.

**Waarschuwing**: wanneer een bestandsshare meer dan 80 is % gebruikt, wordt een *waarschuwing* waarschuwing in het beheerportal: ![voorbeeld: waarschuwing](media/azure-stack-manage-storage-shares/alert-warning.png)


**Kritieke**: wanneer een bestandsshare meer dan 90 is % gebruikt, wordt een *kritieke* waarschuwing in het beheerportal: ![voorbeeld: kritieke waarschuwing](media/azure-stack-manage-storage-shares/alert-critical.png)

**Details weergeven**: In het beheerportal kunt u de details voor een waarschuwing om risicobeperking opties weer te openen: ![voorbeeld: Waarschuwingsdetails weergeven](media/azure-stack-manage-storage-shares/alert-details.png)


## <a name="manage-available-space"></a>Beschikbare ruimte beheren
Wanneer dit nodig is tot vrije ruimte op een share, eerst de minste Invasief methoden gebruiken. Probeer bijvoorbeeld om ruimte te claimen voordat u kiest voor het migreren van een container.  

### <a name="reclaim-capacity"></a>Capaciteit vrijmaken
*Deze optie is van toepassing op implementaties met meerdere knooppunten en de Azure-Stack Development Kit.*

U kunt vrijmaken de capaciteit die wordt gebruikt door de tenantaccounts die zijn verwijderd. Deze capaciteit is automatisch vrijgemaakt wanneer de gegevens [bewaarperiode](azure-stack-manage-storage-accounts.md#set-the-retention-period) is bereikt, of u kan fungeren als u wilt onmiddellijk vrijmaken.

Zie voor meer informatie [vrijmaken capaciteit](azure-stack-manage-storage-accounts.md#reclaim) in de storage-resources beheren.

### <a name="migrate-a-container-between-volumes"></a>Migreren van een container tussen volumes
*Deze optie geldt alleen voor implementaties met meerdere knooppunten.*

Als gevolg van de gebruikspatronen van de tenant gebruik tenant shares meer ruimte dan andere. Het resultaat zijn dat een share die op afstand voor andere shares die relatief ongebruikte zijn lage wordt uitgevoerd.

U kunt ruimte vrijmaken op een overbelaste delen door het handmatig migreren van sommige blob-containers naar een andere share. U kunt verschillende kleinere containers migreren naar een afzonderlijke share die groot genoeg voor alle heeft. U kunt migratie gebruiken om te verplaatsen *gratis* containers. Gratis containers zijn containers die een schijf voor een virtuele machine niet bevatten.   

Migratie legt een containers blob op de nieuwe share.

- Als een container overloop modus heeft ingevoerd en blobs op extra volumes heeft geplaatst, moet de nieuwe share onvoldoende capaciteit om te kunnen bevatten van de blobs voor de container die u migreert hebben. Dit omvat de blobs die op extra shares bevinden zich.

- De PowerShell-cmdlet *Get-AzsStorageContainer* identificeert alleen de ruimte op het oorspronkelijke volume gebruikt voor een container. De cmdlet geeft niet de ruimte die wordt gebruikt door de blobs in de extra volumes plaatsen. De volledige grootte van een container kan daarom niet zijn duidelijk naar voren. Het is mogelijk dat consolidatie van een container op een nieuwe share verzenden die nieuwe share in een overloop waar deze gegevens naar extra shares geplaatst. Als gevolg hiervan moet u mogelijk opnieuw shares opnieuw verdelen.

- Als u niet over machtigingen aan een resourcegroep en niet PowerShell gebruiken om op te vragen van de extra volumes voor overloop gegevens, samen met de eigenaar van deze resourcegroepen en containers te begrijpen van de totale grootte van gegevens te migreren vóór de migratie van die gegevens.  

> [!IMPORTANT]
> Migratie van BLOB's voor een container is een offline-bewerking die het gebruik van PowerShell vereist. Totdat de migratie is voltooid, worden alle blobs voor de container die u migreert offline blijven en kunnen niet worden gebruikt.

#### <a name="to-migrate-containers-using-powershell"></a>Voor het migreren van containers met behulp van PowerShell
1. Bevestigen dat u [Azure PowerShell is geïnstalleerd en geconfigureerd](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Zie [Azure PowerShell gebruiken met Azure Resource Manager](http://go.microsoft.com/fwlink/?LinkId=394767) voor meer informatie.
2.  Bekijk de container die u wilt weten welke gegevens op de share die u wilt migreren. Als u wilt de beste kandidaat containers voor migratie in een volume hebt geïdentificeerd, gebruiken de **Get-AzsStorageContainer** cmdlet:

    ```
    $shares = Get-AzsStorageShare
    $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -Intent Migration
    ```
    Controleer $containers:
    ```
    $containers
    ```
    ![Voorbeeld: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3.  Identificeer de beste bestemming shares voor het opslaan van de container die u migreert:
    ```
    $destinationshares = Get-AzsStorageShare -SourceShareName
    $shares[0].ShareName -Intent ContainerMigration
    ```
    Controleer $destinationshares:
    ```
    $destinationshares
    ```    
    ![Voorbeeld: $destination shares](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. Start de migratie voor een container. Migratie is asynchroon. Als u migratie van aanvullende containers starten voordat de eerste migratie is voltooid, gebruikt u de taak-id voor het bijhouden van de status van elke.
  ```
  $jobId = Start-AzsStorageContainerMigration -ContainerToMigrate $containers[1] -DestinationShareUncPath $destinationshares[0].UncPath
  ```
  Controleer $jobId. Vervang in het volgende voorbeeld wordt *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* met de taak-id die u wilt onderzoeken:
  ```
  $jobId
  d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
  ```
5. De taak-id gebruiken om te controleren op de status van de migratietaak. Als de migratie van de container voltooid is, **MigrationStatus** is ingesteld op **Complete**.
  ```
  Get-AzsStorageContainerMigrationStatus -JobId $jobId
  ```
  ![Voorbeeld: Migratiestatus](media/azure-stack-manage-storage-shares/migration-status1.png)

6.  U kunt een migratietaak in voortgang annuleren. Geannuleerd migratie taken asynchroon worden verwerkt. U kunt de annulering bijhouden met behulp van $jobid:

  ```
  Stop-AzsStorageContainerMigration -JobId $jobId
  ```
  ![Voorbeeld: Terugdraaien van de status](media/azure-stack-manage-storage-shares/rollback.png)

7. U kunt de opdracht in stap 6 opnieuw uitvoeren, totdat de status wordt bevestigd dat de migratietaak **geannuleerd**:  
    ![Voorbeeld: Status geannuleerd](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>VM-schijven verplaatsen
*Deze optie geldt alleen voor implementaties met meerdere knooppunten.*

De meest extreme methode voor het beheren van de ruimte omvat de verplaatsing van virtuele-machineschijven. Omdat het verplaatsen van een gekoppelde container (een bestand met een VM-schijf) complex is, moet u contact op met Microsoft Support om uit te voeren met deze actie.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [virtuele machines aanbieden aan gebruikers](azure-stack-tutorial-tenant-vm.md).
