---
title: Beheren voor de opslagcapaciteit in Azure Stack | Microsoft Docs
description: Controleren en beheren van beschikbare opslagruimte voor Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 01/22/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: 97cdae49b4676500e29ac25b12712c94e575e5f8
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55960561"
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Opslagcapaciteit voor Azure Stack beheren 

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

De informatie in dit artikel helpt bij de Azure Stack cloud operator bewaken en beheren van de opslagcapaciteit van hun Azure Stack-implementatie. De Azure Stack-opslaginfrastructuur wijst een subset van de totale opslagcapaciteit van de Azure Stack-implementatie moet worden gebruikt voor **opslagservices**. De storage-services opslaan van de gegevens van een tenant in shares op volumes die met de knooppunten van de implementatie overeenkomen.

Als een cloud-operator hebt u een beperkte hoeveelheid van opslag aan het werken met. De hoeveelheid opslag wordt gedefinieerd door de oplossing die te implementeren. Uw oplossing wordt geboden door de leverancier van uw OEM wanneer u een oplossing met meerdere knooppunten, of door de hardware waarop u de Azure Stack Development Kit installeren.

Omdat Azure Stack biedt geen ondersteuning voor uitbreiding van de opslagcapaciteit, het is belangrijk om [monitor](#monitor-shares) de beschikbare opslag om ervoor te zorgen voor een efficiënte werking worden bijgehouden.  

Wanneer de resterende beschikbare capaciteit van een share beperkt wordt, wilt [ruimte beheren](#manage-available-space) om te voorkomen dat de shares die worden uitgevoerd uit capaciteit.

Opties voor het beheren van capaciteit zijn onder andere:
- Capaciteit vrijmaken
- Migreren van een container

Wanneer een share is 100% gebruikt, de opslag-service niet meer functies voor deze share. Als hulp bij het terugzetten van bewerkingen voor de share, neem contact op met Microsoft ondersteuning.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Inzicht in volumes en shares, containers en schijven
### <a name="volumes-and-shares"></a>Volumes en shares
De *opslagservice* partities van de beschikbare opslag in afzonderlijke en gelijk volumes die zijn toegewezen voor het opslaan van gegevens van de tenant. Het aantal volumes is gelijk aan het aantal knooppunten in de Azure Stack-implementatie:

- Er zijn vier volumes in een implementatie met vier knooppunten. Elk volume heeft een afzonderlijke share. Op een implementatie met meerdere knooppunten, het aantal shares niet als een knooppunt verwijderd of niet-functionerende is vermindert.
- Als u de Azure Stack Developer Kit gebruikt, is er één volume met één share.

Omdat de storage-service-shares voor exclusief gebruik van storage-services zijn, moet u niet rechtstreeks wijzigen, toevoegen of de bestanden op de shares verwijderen. Storage-services moeten op de bestanden die zijn opgeslagen in deze volumes werken.

Shares op volumes bevatten gegevens van de tenant. Gegevens van de tenant bevat pagina-blobs, blok-blobs, toevoeg-blobs, tabellen, wachtrijen, -databases en gerelateerde metagegevensopslag. Omdat de storage-objecten (blobs, enzovoort) afzonderlijk in één share opgenomen worden, kan de maximale grootte van elk object kan niet groter zijn dan de grootte van een share. De maximale grootte van nieuwe objecten zijn afhankelijk van de capaciteit die nog in een share als niet-gebruikte ruimte wanneer dat nieuwe object wordt gemaakt.

Wanneer een share is onvoldoende vrije ruimte en acties voor [vrijmaken](#reclaim-capacity) ruimte niet geslaagd, of beschikbaar zijn, de Azure Stack-cloud-operator de blob-containers kunt migreren van een share naar een andere.

- Zie voor meer informatie over containers en blobs [Blob storage](azure-stack-key-features.md#blob-storage) in sleutel-functies en concepten in Azure Stack.
- Zie voor meer informatie over de werking van tenantgebruikers met blob storage in Azure Stack [Azure Stack-opslagservices](/azure/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Containers
Tenantgebruikers maken containers die vervolgens worden gebruikt voor het opslaan van blob-gegevens. Terwijl de gebruiker in waarop de container besluit te plaatsen, blobs, de storage-service een algoritme gebruikt om te bepalen op welk volume de container te plaatsen. Het algoritme wordt meestal gekozen voor het volume met de meeste beschikbare ruimte.  

Nadat een blob in een container wordt geplaatst, kan die blob worden uitgebreid om meer ruimte gebruiken. Als u nieuwe blobs en bestaande toevoegen groeien blobs, de beschikbare ruimte op het volume waarin deze container wordt verkleind.  

Containers zijn niet beperkt tot één share. Wanneer de gecombineerde blob-gegevens in een container gebruik 80% of meer van de beschikbare ruimte groeit, de container ingevoerd *overloop* modus. In de modus overloop, worden alle nieuwe blobs die zijn gemaakt in die container naar een ander volume met voldoende ruimte toegewezen. Na verloop van tijd kan een container in de modus voor overloop blobs die zijn verdeeld over meerdere volumes hebben.

Wanneer 80% en 90% van de beschikbare ruimte in een volume wordt gebruikt, genereert het systeem waarschuwingen in de Azure Stack-beheerdersportal. Cloudoperators Controleer beschikbare opslagcapaciteit, en van plan bent de inhoud opnieuw verdelen. De storage-service niet meer functioneert als een schijf 100 is % gebruikt en er geen extra waarschuwingen worden gegenereerd.

### <a name="disks"></a>Disks
VM-schijven worden toegevoegd aan containers door tenants en de schijf van een besturingssysteem bevatten. Virtuele machines kunnen ook een of meer gegevensschijven hebben. Beide typen schijven worden opgeslagen als pagina-blobs. De richtlijnen voor tenants is om elke schijf in een afzonderlijke container om de prestaties van de virtuele machine te verbeteren.
- Elke container met een schijf (pagina-blobs) van een virtuele machine wordt beschouwd als een gekoppelde-container met de virtuele machine die eigenaar is van de schijf.
- Een container waarop geen een schijf van een virtuele machine wordt beschouwd als een gratis container.

De opties voor de vrije ruimte op een gekoppelde container [zijn beperkt](#move-vm-disks).
> [!TIP]  
> Cloudoperators beheren schijven die zijn gekoppeld aan virtuele machines (VM's) die tenants aan een container toevoegen kunnen niet rechtstreeks. Wanneer u plant voor het beheren van ruimte op storage-shares, kan het echter zijn gebruikt om te begrijpen hoe schijven in verband met containers en -shares.

## <a name="monitor-shares"></a>Monitor shares
Gebruik PowerShell of de admin-portal voor het bewaken van shares, zodat u inzicht in wanneer de vrije ruimte is beperkt. Wanneer u de portal gebruiken, ontvangt u meldingen over shares die weinig schijfruimte.    

### <a name="use-powershell"></a>PowerShell gebruiken
Als een cloud-operator, kunt u de opslagcapaciteit van een share met behulp van PowerShell controleren **Get-AzsStorageShare** cmdlet. De cmdlet Get-AzsStorageShare retourneert het totale aantal toegewezen en vrije ruimte in bytes op elk van de shares.   
![Voorbeeld: Vrije ruimte voor shares retourneren](media/azure-stack-manage-storage-shares/free-space.png)

- **Totale capaciteit** is de totale ruimte in bytes die beschikbaar op de share zijn. Deze ruimte wordt gebruikt voor gegevens en metagegevens die wordt beheerd door de storage-services.
- **Capaciteitsplanning** is de hoeveelheid gegevens in bytes die wordt gebruikt door alle gebieden van de bestanden die de gegevens van de tenant en gekoppelde metagegevens opgeslagen.

### <a name="use-the-administrator-portal"></a>De beheerdersportal gebruiken
Als een cloud-operator, kunt u de beheerportal om de opslagcapaciteit van alle shares weer te geven.

1. Aanmelden bij de [-beheerportal](https://adminportal.local.azurestack.external).
2. Selecteer **alle services** > **opslag** openen van de lijst met de bestandsshare waar u de informatie over het gebruik kunt bekijken. 

    ![Voorbeeld: Bestandsshares voor opslag](media/azure-stack-manage-storage-shares/storage-file-shares.png)

  - **Totaal aantal** is de totale ruimte in bytes die beschikbaar op de share zijn. Deze ruimte wordt gebruikt voor gegevens en metagegevens die wordt beheerd door de storage-services.
  - **GEBRUIKT** is de hoeveelheid gegevens in bytes die wordt gebruikt door alle gebieden van de bestanden die de gegevens van de tenant en gekoppelde metagegevens opgeslagen.

### <a name="storage-space-alerts"></a>Storage space waarschuwingen
Wanneer u de beheerportal gebruikt, ontvangt u meldingen over shares die weinig schijfruimte.

> [!IMPORTANT]
> Als een cloud-operator, houd u shares volledig gebruik wordt bereikt. Wanneer een share is 100% gebruikt, de opslag-service niet meer functies voor deze share. Als u wilt herstellen van de vrije ruimte en herstelbewerkingen op een share die 100% gebruikt, moet u contact op met Microsoft ondersteuning.

**Waarschuwing**: Wanneer een bestandsshare meer dan 80 is % gebruikt, ontvangt u een *waarschuwing* waarschuwing in het beheerportal: ![Voorbeeld: Waarschuwing](media/azure-stack-manage-storage-shares/alert-warning.png)


**Kritieke**: Wanneer een bestandsshare meer dan 90 is % gebruikt, ontvangt u een *kritieke* waarschuwing in het beheerportal: ![Voorbeeld: Kritieke waarschuwing](media/azure-stack-manage-storage-shares/alert-critical.png)

**Details weergeven**: U kunt de details van een waarschuwing om weer te geven van de opties voor risicobeperking openen in de beheerportal: ![Voorbeeld: Waarschuwingsdetails weergeven](media/azure-stack-manage-storage-shares/alert-details.png)


## <a name="manage-available-space"></a>Beschikbare ruimte beheren
Wanneer deze die nodig zijn voor de vrije ruimte op een share is, eerst de minste Invasief methoden gebruiken. Bijvoorbeeld, probeert het vrijmaken van ruimte voordat u kiest voor het migreren van een container.  

### <a name="reclaim-capacity"></a>Capaciteit vrijmaken
*Deze optie is van toepassing op zowel implementaties met meerdere knooppunten en de Azure Stack Development Kit.*

U kunt de capaciteit die wordt gebruikt door tenantaccounts die zijn verwijderd vrijmaken. Deze capaciteit automatisch wordt vrijgemaakt als de gegevens [bewaarperiode](azure-stack-manage-storage-accounts.md#set-the-retention-period) is bereikt, of u actie kunt ondernemen om onmiddellijk vrijmaken.

Zie voor meer informatie, [vrijmaken capaciteit](azure-stack-manage-storage-accounts.md#reclaim) in de storage-resources beheren.

### <a name="migrate-a-container-between-volumes"></a>Een container tussen volumes migreren
*Deze optie geldt alleen voor implementaties met meerdere knooppunten.*

Vanwege de gebruikspatronen van de tenant gebruiken sommige bestandsshares tenant meer ruimte dan andere. Het resultaat is een share die op ruimte vóór andere shares die relatief ongebruikte zijn laag wordt uitgevoerd.

U kunt ruimte vrijmaken op een overbelaste delen door het handmatig migreren sommige blob-containers naar een andere share. U kunt verschillende kleinere containers migreren naar een afzonderlijke share met capaciteit voor het opslaan van alle. U kunt migratie gebruiken om te verplaatsen *gratis* containers. Gratis containers zijn containers die geen een schijf voor een virtuele machine bevatten.   

Migratie consolideert alle een blob van containers op de nieuwe share.

- Als een container overloop modus heeft ingevoerd en blobs op extra volumes is geplaatst, is de nieuwe share moet voldoende capaciteit voor alle van de blobs voor de container die u migreert. Dit omvat de blobs die op extra shares bevinden zich.

- De PowerShell-cmdlet *Get-AzsStorageContainer* geeft alleen de ruimte in gebruik is op het oorspronkelijke volume voor een container. De cmdlet geeft niet de ruimte die wordt gebruikt door blobs op extra volumes plaatsen. De volledige grootte van een container kan daarom niet zijn duidelijk. Het is mogelijk dat consolidatie van een container op een nieuwe share kan verzenden die nieuwe share in een overloop waar deze gegevens naar extra shares wordt geplaatst. Als gevolg hiervan moet u mogelijk opnieuw shares opnieuw verdelen.

- Als u niet over machtigingen voor een resourcegroep en PowerShell niet meer gebruiken om op te vragen van de extra volumes voor overloop gegevens, samen met de eigenaar van deze resourcegroepen en containers te begrijpen van de totale grootte van gegevens te migreren voordat u migreert van die gegevens.  

> [!IMPORTANT]
> Migratie van blobs voor een container is een offline-bewerking die het gebruik van PowerShell vereist. Totdat de migratie is voltooid, wordt alle blobs voor de container die u wilt migreren offline blijven en kunnen niet worden gebruikt. U dient ook te vermijden upgraden van Azure Stack totdat alle doorlopende migratie is voltooid.

#### <a name="to-migrate-containers-using-powershell"></a>Voor het migreren van containers met behulp van PowerShell
1. Bevestigen dat u hebt [Azure PowerShell is geïnstalleerd en geconfigureerd](https://azure.microsoft.com/documentation/articles/powershell-install-configure/). Zie [Azure PowerShell gebruiken met Azure Resource Manager](https://go.microsoft.com/fwlink/?LinkId=394767) voor meer informatie.
2.  Controleer de container voor meer informatie over welke gegevens staan op de share die u wilt migreren. Voor het identificeren van de beste kandidaat containers voor migratie in een volume, gebruikt u de **Get-AzsStorageContainer** cmdlet:

    ```PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    $shares = Get-AzsStorageShare -FarmName $farm_name
    $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -FarmName $farm_name
    ```
    Controleer $containers:

    ```PowerShell
    $containers
    ```

    ![Voorbeeld: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3.  Identificeert de beste bestemming shares voor het opslaan van de container die u migreert:

    ```PowerShell
    $destinationshares = Get-AzsStorageShare -SourceShareName
    $shares[0].ShareName -Intent ContainerMigration
    ```

    Controleer $destinationshares:

    ```PowerShell 
    $destinationshares
    ```

    ![Voorbeeld: $destination shares](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. De migratie voor een container starten. Migratie is asynchroon. Als u de migratie van andere containers starten voordat de eerste migratie is voltooid, gebruikt u de taak-id voor het bijhouden van de status van elk.

  ```PowerShell
  $job_id = Start-AzsStorageContainerMigration -StorageAccountName $containers[0].Accountname -ContainerName $containers[0].Containername -ShareName $containers[0].Sharename -DestinationShareUncPath $destinationshares[0].UncPath -FarmName $farm_name
  ```

  Vervolgens $jobId te onderzoeken. Vervang in het volgende voorbeeld *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* met de taak-id die u wilt bekijken:

  ```PowerShell
  $jobId
  d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
  ```

5. De taak-id gebruiken om te controleren op de status van de migratietaak. Als de container-migratie voltooid is, **MigrationStatus** is ingesteld op **voltooid**.

  ```PowerShell 
  Get-AzsStorageContainerMigrationStatus -JobId $job_id -FarmName $farm_name
  ```

  ![Voorbeeld: Migratiestatus](media/azure-stack-manage-storage-shares/migration-status1.png)

6.  U kunt een migratietaak in uitvoering annuleren. Geannuleerd migratie taken asynchroon worden verwerkt. U kunt de annulering bijhouden met behulp van $jobid:

  ```PowerShell
  Stop-AzsStorageContainerMigration -JobId $job_id -FarmName $farm_name
  ```

  ![Voorbeeld: Van de terugdraaistatus](media/azure-stack-manage-storage-shares/rollback.png)

7. U kunt de opdracht in stap 6 opnieuw uitvoeren, totdat de status wordt bevestigd dat de migratietaak is **geannuleerd**:  

    ![Voorbeeld: Status geannuleerd](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>VM-schijven verplaatsen
*Deze optie geldt alleen voor implementaties met meerdere knooppunten.*

De meest extreme methode voor het beheren van de ruimte omvat de verplaatsing van virtuele-machineschijven. Omdat het verplaatsen van een gekoppelde container (een bestand met een VM-schijf) complex is, moet u contact op met Microsoft Support om uit te voeren van deze actie.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [virtuele machines bieden aan gebruikers](azure-stack-tutorial-tenant-vm.md).
