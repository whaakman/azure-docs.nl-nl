---
title: Migratie van VM's naar Azure Premium Storage | Microsoft Docs
description: Uw bestaande virtuele machines migreren naar Azure Premium Storage. Premium Storage biedt ondersteuning voor schijven voor hoge prestaties en lage latentie voor I/O-intensieve werkbelastingen op Azure Virtual Machines.
services: storage
author: yuemlu
ms.service: storage
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.subservice: common
ms.openlocfilehash: d42183e1db49850afc115fcb5645baf7290cf3c8
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477592"
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a>Migreren naar Azure Premium Storage (niet-beheerde schijven)

> [!NOTE]
> In dit artikel wordt beschreven hoe u migreert van een virtuele machine die gebruikmaakt van niet-beheerde standaardschijven aan een virtuele machine die gebruikmaakt van niet-beheerde premium-schijven. U wordt aangeraden dat u Azure Managed Disks gebruiken voor nieuwe virtuele machines en uw vorige niet-beheerde schijven te converteren naar managed disks. Beheerde schijven ingang de onderliggende opslagaccounts, zodat u niet te hoeven. Zie voor meer informatie onze [overzicht van Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).
>

Azure Premium Storage voorziet in ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines waarop I/O-intensieve werkbelastingen worden uitgevoerd. VM-schijven van uw toepassing migreren naar Azure Premium Storage kunt u profiteren van de snelheid en prestaties van deze schijven uitvoeren.

Het doel van deze handleiding is om nieuwe gebruikers van Azure Premium Storage betere voorbereidingen voor het maken van een probleemloze overgang van hun huidige systeem naar Premium Storage te helpen. De handleiding behandelt drie van de belangrijkste onderdelen van dit proces:

* [Plan voor de migratie naar Premium Storage](#plan-the-migration-to-premium-storage)
* [Voorbereiden en virtuele harde schijven (VHD's) kopiëren naar Premium Storage](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [Met Premium-opslag van Azure virtuele Machine maken](#create-azure-virtual-machine-using-premium-storage)

U kunt virtuele machines vanaf andere platforms migreren naar Azure Premium Storage of bestaande Azure-VM's migreren van standaardopslag naar Premium Storage. Deze handleiding bevat informatie over stappen voor beide twee scenario's. Volg de stappen die zijn opgegeven in de betreffende sectie afhankelijk van uw scenario.

> [!NOTE]
> U vindt een overzicht van functies en prijzen van Premium-opslag in Premium-opslag: [Opslag met hoge prestaties voor Azure Virtual Machine-werkbelasting](../../virtual-machines/windows/premium-storage.md). Het is raadzaam om de migratie van een virtuele machine-schijf vereisen hoge IOPS naar Azure Premium Storage voor de beste prestaties voor uw toepassing. Als de schijf geen hoge IOPS is vereist, kunt u kosten kunt beperken door het onderhoud ervan in de Standard-opslag, waarin de gegevens van de virtuele machine-schijf op hardeschijfstations (HDD's) in plaats van SSD's worden opgeslagen.
>

De migratie voltooien in zijn geheel is mogelijk aanvullende acties zowel vóór als na de stappen in deze handleiding. Voorbeelden hiervan zijn virtuele netwerken of eindpunten configureren of codewijzigingen in de toepassing zelf mogelijk moet hiervoor de enige downtime in uw toepassing. Deze acties zijn uniek voor elke toepassing en u ze samen met de stappen in deze handleiding voor de volledige overgang naar Premium Storage zo weinig mogelijk moet voltooien.

## <a name="plan-the-migration-to-premium-storage"></a>Plan voor de migratie naar Premium Storage
In deze sectie zorgt u ervoor dat u bent klaar om de migratie stappen in dit artikel helpt u bij het maken van de beste beslissing op schijf en VM-typen.

### <a name="prerequisites"></a>Vereisten
* U moet een Azure-abonnement. Als u niet hebt, kunt u een één maand [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) abonnement of gaat u naar [Azure-prijzen](https://azure.microsoft.com/pricing/) voor meer opties.
* Voor het uitvoeren van PowerShell-cmdlets, moet u de Microsoft Azure PowerShell-module. Zie [How to install and configure Azure PowerShell](/powershell/azure/overview) (Azure PowerShell installeren en configureren) voor het installatiepunt en de installatie-instructies.
* Wanneer u van plan bent te gebruiken Azure-VM's die worden uitgevoerd op de Premium-opslag, moet u de compatibele virtuele machines voor een Premium Storage gebruiken. U kunt schijven van zowel Standard als Premium-opslag gebruiken met Premium Storage kunnen VM's. Premium storage-schijven zijn in de toekomst beschikbaar met meer VM-typen. Zie voor meer informatie over alle beschikbare Azure-VM-schijftypen en grootten [grootten voor virtuele machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) en [groottes voor Cloud Services](../../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Overwegingen
Een Azure-VM ondersteunt meerdere Premium-opslagschijven koppelen zodat uw toepassingen tot 256 TB opslagruimte per virtuele machine hebben kunnen. Met Premium Storage kunnen uw toepassingen maar liefst 80.000 IOPS (invoer/uitvoer-bewerkingen per seconde) per VM en 2000 MB per seconde schijfdoorvoer per VM met extreem lage latenties bij leesbewerkingen. Hebt u opties in een aantal VM's en schijven. In deze sectie is om te helpen u een optie die het beste bij uw werkbelasting weer te geven.

#### <a name="vm-sizes"></a>Formaten van virtuele machines
De specificaties van de grootte van virtuele Azure-machine vindt u in [grootten voor virtuele machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Bekijk de prestatiekenmerken van virtuele machines die werken met Premium Storage en kiest u de meest geschikte VM-grootte die het beste bij uw workload. Zorg ervoor dat er voldoende bandbreedte beschikbaar op de virtuele machine is om het schijfverkeer te stimuleren.

#### <a name="disk-sizes"></a>Schijfformaten
Er zijn vijf typen schijven die kunnen worden gebruikt met de virtuele machine en elk heeft specifieke IOPs en doorvoer limieten. In overweging nemen deze limieten bij het kiezen van het schijftype voor uw virtuele machine op basis van de behoeften van uw toepassing in termen van capaciteit, prestaties, schaalbaarheid en piek wordt geladen.

| Schijftype voor Premium-schijven  | P10   | P20   | P30            | P40            | P50            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| Schijfgrootte           | 128 GB| 512 GB| 1024 GB (1 TB) | 2048 GB (2 TB) | 4095 GB (4 TB) | 
| IOP's per schijf       | 500   | 2300  | 5000           | 7500           | 7500           | 
| Doorvoer per schijf | 100 MB per seconde | 150 MB per seconde | 200 MB per seconde | 250 MB per seconde | 250 MB per seconde |

Te bepalen of er extra gegevensschijven die nodig zijn voor uw virtuele machine zijn afhankelijk van uw workload. U kunt meerdere permanente gegevensschijven koppelen aan uw virtuele machine. Indien nodig, kunt u op de schijven te verhogen van de capaciteit en prestaties van het volume stripe. (Zie Wat is er schijfsegmentering [hier](../../virtual-machines/windows/premium-storage-performance.md#disk-striping).) Als u Premium Storage-gegevensschijven met stripe [opslagruimten][4], moet u deze configureren met één kolom voor elke schijf die wordt gebruikt. De algehele prestaties van het striped volume kan anders zijn lager is dan verwacht vanwege een ongelijke distributie van verkeer voor de schijven. Voor Linux-VM's kunt u de *mdadm* hulpprogramma hiervoor ook. Zie artikel [Software-RAID configureren onder Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) voor meer informatie.

#### <a name="storage-account-scalability-targets"></a>Schaalbaarheidsdoelen voor Storage-account
Premium Storage-accounts hebben de volgende schaalbaarheidsdoelen naast de [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md). Als uw toepassingsvereisten de schaalbaarheidsdoelen van een enkel opslagaccount overschrijdt, uw toepassing gebruik meerdere opslagaccounts maken en partities voor uw gegevens voor deze opslagaccounts.

| Totale capaciteit | Totale bandbreedte voor een Account voor lokaal redundante opslag |
|:--- |:--- |
| Capaciteit van de schijf: 35TB<br />Snapshot-capaciteit: 10 TB |Maximaal 50 gigabits per seconde voor binnenkomend en uitgaand |

Bekijk voor meer informatie over specificaties van Premium Storage [Scalability and Performance Targets bij het gebruik van Premium Storage](../../virtual-machines/windows/premium-storage.md#scalability-and-performance-targets).

#### <a name="disk-caching-policy"></a>Beleid voor caching schijf
Beleid voor caching schijf is standaard *alleen-lezen* voor alle Premium gegevensschijven, en *lezen / schrijven* voor de Premium-schijf die is gekoppeld aan de virtuele machine. Deze configuratieinstelling wordt aanbevolen om de optimale prestaties voor IOs van uw toepassing. Voor schijven schrijfintensief of alleen-schrijven gegevens (zoals SQL Server-logboekbestanden) uitschakelen in schijfcache zodat u betere prestaties van toepassingen kunt bereiken. De cache-instellingen voor bestaande gegevensschijven kunnen worden bijgewerkt met behulp van [Azure Portal](https://portal.azure.com) of de *- HostCaching* parameter van de *Set AzureDataDisk* cmdlet.

#### <a name="location"></a>Locatie
Kies een locatie waar Azure Premium Storage beschikbaar is. Zie [Azure Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over de beschikbare locaties. Virtuele machines zich in dezelfde regio bevinden als het opslagaccount dat winkels de schijven voor de virtuele machine veel betere prestaties krijgt dan als ze zich in verschillende regio's.

#### <a name="other-azure-vm-configuration-settings"></a>Andere Azure-VM-configuratie-instellingen
Bij het maken van een Azure-VM, wordt u gevraagd om bepaalde VM-instellingen te configureren. Denk eraan dat enkele instellingen staan vast voor de levensduur van de virtuele machine, terwijl u kunt wijzigen of andere later toevoegen. Deze Azure-VM-configuratie-instellingen controleren en zorg ervoor dat deze zodat deze overeenkomt met de vereisten van uw workloads op de juiste wijze zijn geconfigureerd.

### <a name="optimization"></a>Optimalisatie
[Azure Premium Storage: Ontwerp voor hoge prestaties](../../virtual-machines/windows/premium-storage-performance.md) bevat richtlijnen voor het bouwen van krachtige toepassingen met behulp van Azure Premium Storage. Volg de richtlijnen in combinatie met aanbevolen procedures voor prestaties van toepassing op de technologieën die worden gebruikt door uw toepassing.

## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Voorbereiden en virtuele harde schijven (VHD's) kopiëren naar Premium Storage
De volgende sectie bevat richtlijnen voor het voorbereiden van VHD's van uw virtuele machine en VHD's kopiëren naar Azure Storage.

* [Scenario 1: "Ik ben bestaande Azure-VM's migreren naar Azure Premium Storage."](#scenario1)
* [Scenario 2: "Ik ben VM's migreren vanaf andere platforms naar Azure Premium Storage."](#scenario2)

### <a name="prerequisites"></a>Vereisten
Als u wilt voorbereiden van de VHD's voor migratie, hebt u het volgende nodig:

* Een Azure-abonnement, een storage-account en een container in het storage-account waarmee u uw VHD kunt kopiëren. Houd er rekening mee dat het doelopslagaccount een Standard of Premium Storage-account, afhankelijk van uw behoefte kan worden.
* Een hulpprogramma voor het generaliseren van de VHD als u van plan bent om te maken van meerdere VM-exemplaren van deze. Bijvoorbeeld, sysprep voor Windows of virt-sysprep voor Ubuntu.
* Een hulpprogramma voor het uploaden van het VHD-bestand naar het opslagaccount. Zie [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md) of gebruik een [Azure storage explorer](https://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Deze handleiding wordt beschreven voor het kopiëren van uw VHD met het AzCopy-hulpprogramma.

> [!NOTE]
> Als u ervoor de optie synchrone kopiëren met AzCopy kiest, voor optimale prestaties, kopieert u uw VHD door het uitvoeren van een van deze hulpprogramma's van een Azure-virtuele machine die zich in dezelfde regio als het doelopslagaccount. Als u een VHD van een Azure-VM in een andere regio kopieert, is het mogelijk dat de prestaties van uw langzamer.
>
> Voor een grote hoeveelheid gegevens te kopiëren over beperkte bandbreedte, kunt u overwegen [gebruik van de Azure Import/Export-service voor het overbrengen van gegevens naar Blob-opslag](../storage-import-export-service.md); Hiermee kunt u om over te dragen van uw gegevens door de verzending van harde schijven naar een Azure-datacenter. U kunt de Azure Import/Export-service gebruiken om gegevens te kopiëren naar een standard storage-account alleen. Zodra de gegevens zich in uw standard storage-account, kunt u een gebruiken de [kopie Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) of AzCopy om over te dragen van de gegevens naar uw premium storage-account.
>
> Houd er rekening mee dat Microsoft Azure biedt alleen ondersteuning voor vaste grootte VHD-bestanden. VHDX-bestanden of dynamische VHD's worden niet ondersteund. Hebt u een dynamische VHD, kunt u deze converteren naar vaste grootte met behulp van de [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) cmdlet.
>
>

### <a name="scenario1"></a>Scenario 1: "Ik ben bestaande Azure-VM's migreren naar Azure Premium Storage."
Als u bestaande Azure-VM's migreert, moet de virtuele machine stoppen, VHD's per het type VHD die u wilt voorbereiden en kopieer vervolgens de VHD met het AzCopy- of PowerShell.

De virtuele machine moet volledig omlaag voor het migreren van een oude toestand. Er zijn een uitvaltijd totdat de migratie is voltooid.

#### <a name="step-1-prepare-vhds-for-migration"></a>Step 1. VHD voorbereiden voor migratie
Als u een bestaande Azure-VM's naar Premium Storage migreert, wordt uw VHD kan zijn:

* Een gegeneraliseerde installatiekopie
* Een unieke besturingssysteemschijf
* Een gegevensschijf

Hieronder helpen we u bij deze 3 scenario's voor het voorbereiden van uw VHD.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Een gegeneraliseerde VHD met besturingssysteem gebruiken voor het maken van meerdere VM-exemplaren
Als u een VHD die wordt gebruikt voor het maken van meerdere algemene Azure-VM-exemplaren uploaden wilt, moet u eerst de VHD met een hulpprogramma sysprep generaliseren. Dit is van toepassing op een VHD die on-premises of in de cloud. Sysprep verwijdert alle systeemspecifieke gegevens uit de VHD.

> [!IMPORTANT]
> Een momentopname of back-up van uw virtuele machine voordat u het generaliseren. Actieve sysprep gestopt en toewijzing ongedaan maken van het VM-exemplaar. Volg onderstaande stappen om sysprep de VHD van een Windows-besturingssysteem. Houd er rekening mee dat de Sysprep wordt uitgevoerd, moet u de virtuele machine afgesloten. Zie voor meer informatie over Sysprep [Sysprep overzicht](https://technet.microsoft.com/library/hh825209.aspx) of [technische documentatie van Sysprep](https://technet.microsoft.com/library/cc766049.aspx).
>
>

1. Open een opdrachtpromptvenster als beheerder.
2. Voer de volgende opdracht Sysprep openen:

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. Selecteer in het hulpprogramma voor systeemvoorbereiding, voer System Out-of-Box Experience (OOBE), schakel het selectievakje generaliseren, selecteer **afsluiten**, en klik vervolgens op **OK**, zoals wordt weergegeven in de onderstaande afbeelding. Sysprep wordt generaliseren van het besturingssysteem en het systeem afsluiten.

    ![][1]

Gebruik virt sysprep hiervoor ook voor een Ubuntu-VM. Zie [virt sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) voor meer informatie. Zie ook enkele van de open-source [inrichten van Linux-Server software](http://www.cyberciti.biz/tips/server-provisioning-software.html) voor andere Linux-besturingssystemen.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Een unieke VHD met besturingssysteem gebruiken voor het maken van één VM-instantie
Hebt u een toepassing die wordt uitgevoerd op de virtuele machine waarvoor u de specifieke gegevens van de machine, u de VHD niet generaliseren. Een niet-gegeneraliseerde VHD kan worden gebruikt om een unieke virtuele Azure-machine-exemplaar te maken. Bijvoorbeeld, als u een domeincontroller op uw VHD hebt, maakt sysprep wordt uitgevoerd het niet effectief als een domeincontroller. Bekijk de toepassingen die worden uitgevoerd op de virtuele machine en de impact van het sysprep uitvoeren op deze voordat u de VHD te generaliseren.

##### <a name="register-data-disk-vhd"></a>Registreren van de VHD met gegevensschijf
Hebt u gegevensschijven in Azure worden gemigreerd, moet u controleren of de VM's die gebruikmaken van de gegevensschijven van deze worden afgesloten.

Volg de stappen die hieronder worden beschreven VHD kopiëren naar Azure Premium Storage en registreert u dit als een schijf ingerichte gegevens.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. Het doel van uw VHD maken
Maak een opslagaccount voor het onderhouden van uw VHD's. Houd rekening met de volgende punten bij het plannen waar uw VHD's opgeslagen:

* Het doel van Premium storage-account.
* Locatie van het opslagaccount moet hetzelfde zijn als Premium-opslag kunnen Azure-VM's u in de laatste fase maakt. U kunt kopiëren naar een nieuw opslagaccount of een abonnement om te gebruiken hetzelfde opslagaccount op basis van uw behoeften.
* Kopieer en bewaar de opslagaccountsleutel van het doelopslagaccount voor de volgende fase.

Een gegevensschijf kunt u enkele gegevensschijven los bewaren in een standard storage-account (bijvoorbeeld schijven met minder dynamische opslag), maar we raden u om alle gegevens voor productie-werkbelasting gebruik van premium-opslag te verplaatsen.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Stap 3. Kopieer de VHD met het AzCopy- of PowerShell
U moet uw container pad en opslagaccountsleutel voor het verwerken van een van deze twee opties gevonden. Container pad en de opslagaccountsleutel vindt u **Azure Portal** > **opslag**. De URL wordt dan zoals container 'https://myaccount.blob.core.windows.net/mycontainer/'.

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Optie 1: Kopieer geen VHD met AzCopy (asynchrone kopiëren)
Met behulp van AzCopy, u kunt eenvoudig de VHD uploaden via Internet. Dit kan tijd duren, afhankelijk van de grootte van de VHD's. Vergeet niet om te controleren of limieten Inkomend/uitgaand verkeer van het opslagaccount wanneer u deze optie. Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie.

1. Download en installeer AzCopy hier: [Meest recente versie van AzCopy](https://aka.ms/downloadazcopy)
2. Open Azure PowerShell en Ga naar de map waarin AzCopy wordt geïnstalleerd.
3. Gebruik de volgende opdracht uit om te kopiëren van de VHD-bestand van 'Source' naar 'Doel'.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Voorbeeld:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    Hier volgen beschrijvingen van de parameters in de AzCopy-opdracht:

   * **/ Bron:  *&lt;bron&gt;:*** Locatie van de map of de URL van de opslag-container met de VHD.
   * **/SourceKey: *&lt;source-account-key&gt;:*** Opslagaccountsleutel van de bron-storage-account.
   * **/ Dest:  *&lt;bestemming&gt;:*** Opslag container-URL te kopiëren van de VHD op.
   * **/ DestKey:  *&lt;dest accountsleutel&gt;:*** Opslagaccountsleutel van het doelopslagaccount.
   * **/ Patroon:  *&lt;-bestandsnaam&gt;:*** Geef de bestandsnaam op van de VHD te kopiëren.

Voor meer informatie over het gebruik van AzCopy hulpprogramma [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Optie 2: Kopieer geen VHD met PowerShell (Synchronized kopiëren)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U kunt ook het VHD-bestand met de PowerShell-cmdlet Start-AzStorageBlobCopy kopiëren. Gebruik de volgende opdracht uit op Azure PowerShell om te kopiëren van de VHD. Vervang de waarden in <> met overeenkomende waarden van uw bron- en storage-account. Voor het gebruik van deze opdracht moet u een container met de naam VHD's in uw doel-opslagaccount hebben. Als de container niet bestaat, maakt voordat u de opdracht uitvoert.

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

Voorbeeld:

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <a name="scenario2"></a>Scenario 2: "Ik ben VM's migreren vanaf andere platforms naar Azure Premium Storage."
Als u VHD van niet - Azure-Cloudopslag in Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Het pad van de volledige broncode van de lokale directory waar VHD is opgeslagen bij de hand hebt, en vervolgens met behulp van AzCopy te uploaden naar Azure Storage.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Step 1. VHD naar een lokale map exporteren
##### <a name="copy-a-vhd-from-aws"></a>Kopieer een VHD van AWS
1. Als u van AWS gebruikmaakt, exporteert u de EC2-instantie op een VHD in een Amazon S3-bucket. Volg de stappen beschreven in de Amazon-documentatie voor Amazon EC2-instanties exporteren van het hulpprogramma Amazon EC2-opdrachtregelinterface (CLI) installeren en uitvoeren van de opdracht instantie-export-taak maken voor het exporteren van de EC2-instantie naar een VHD-bestand. Zorg ervoor dat u **VHD** voor de schijf&#95;INSTALLATIEKOPIE&#95;indeling variabele bij het uitvoeren van de **-exemplaar-export-taak maken** opdracht. De geëxporteerde VHD-bestand wordt opgeslagen in de Amazon S3-bucket die u tijdens dat proces opgeeft.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. Download het VHD-bestand van de S3-bucket. Selecteer vervolgens het VHD-bestand, **acties** > **downloaden**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopieer een VHD van andere niet-Azure-cloud
Als u VHD van niet - Azure-Cloudopslag in Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Kopieer het pad van de volledige broncode van de lokale directory waar de VHD is opgeslagen.

##### <a name="copy-a-vhd-from-on-premises"></a>Kopieer geen VHD van on-premises
Als u VHD vanuit een on-premises-omgeving migreert, moet u de volledige broncode pad waar de VHD is opgeslagen. Het bronpad kan een server of bestandsshare zijn.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. Het doel van uw VHD maken
Maak een opslagaccount voor het onderhouden van uw VHD's. Houd rekening met de volgende punten bij het plannen waar uw VHD's opgeslagen:

* Het doelopslagaccount mogelijk standard of premium storage, afhankelijk van de vereisten van uw toepassing.
* De regio van opslagaccount moet hetzelfde zijn als Premium-opslag kunnen Azure-VM's u in de laatste fase maakt. U kunt kopiëren naar een nieuw opslagaccount of een abonnement om te gebruiken hetzelfde opslagaccount op basis van uw behoeften.
* Kopieer en bewaar de opslagaccountsleutel van het doelopslagaccount voor de volgende fase.

We raden u om alle gegevens voor productie-werkbelasting gebruik van premium-opslag te verplaatsen.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Stap 3. De VHD uploaden naar Azure Storage
Nu dat u uw VHD in de lokale map hebt, kunt u AzCopy of AzurePowerShell het VHD-bestand uploaden naar Azure Storage. Beide opties vindt u hier:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Optie 1: Met behulp van Azure PowerShell Add-AzureVhd het VHD-bestand uploaden

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

Een voorbeeld <Uri> mogelijk ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***. Een voorbeeld <FileInfo> mogelijk ***"C:\path\to\upload.vhd"***.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Optie 2: AzCopy gebruiken om de VHD-bestand te uploaden
Met behulp van AzCopy, u kunt eenvoudig de VHD uploaden via Internet. Dit kan tijd duren, afhankelijk van de grootte van de VHD's. Vergeet niet om te controleren of limieten Inkomend/uitgaand verkeer van het opslagaccount wanneer u deze optie. Zie [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) voor meer informatie.

1. Download en installeer AzCopy hier: [Meest recente versie van AzCopy](https://aka.ms/downloadazcopy)
2. Open Azure PowerShell en Ga naar de map waarin AzCopy wordt geïnstalleerd.
3. Gebruik de volgende opdracht uit om te kopiëren van de VHD-bestand van 'Source' naar 'Doel'.

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    Voorbeeld:

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    Hier volgen beschrijvingen van de parameters in de AzCopy-opdracht:

   * **/ Bron:  *&lt;bron&gt;:*** Locatie van de map of de URL van de opslag-container met de VHD.
   * **/SourceKey: *&lt;source-account-key&gt;:*** Opslagaccountsleutel van de bron-storage-account.
   * **/ Dest:  *&lt;bestemming&gt;:*** Opslag container-URL te kopiëren van de VHD op.
   * **/ DestKey:  *&lt;dest accountsleutel&gt;:*** Opslagaccountsleutel van het doelopslagaccount.
   * **/ BlobType: pagina:** Geeft aan dat de bestemming een pagina-blob.
   * **/ Patroon:  *&lt;-bestandsnaam&gt;:*** Geef de bestandsnaam op van de VHD te kopiëren.

Voor meer informatie over het gebruik van AzCopy hulpprogramma [gegevensoverdracht met het AzCopy-opdrachtregelprogramma](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD
U kunt ook een VHD uploaden naar uw opslagaccount met behulp van een van de volgende manieren:

* [Azure-opslag kopiëren van de Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [Azure Storage Explorer uploaden van Blobs](https://azurestorageexplorer.codeplex.com/)
* [Naslaginformatie voor Storage Import/Export Service REST API](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> U wordt aangeraden met Import/Export-Service als de geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) om in te schatten van de tijd van de eenheid grootte en de overdracht van gegevens.
>
> Import/Export kan worden gebruikt om te kopiëren naar een standard storage-account. U wilt kopiëren van standaardopslag naar premium storage-account met behulp van een hulpprogramma zoals AzCopy.
>
>

## <a name="create-azure-virtual-machine-using-premium-storage"></a>Maken van virtuele Azure-machines met behulp van Premium Storage
Nadat de VHD wordt geüpload naar of gekopieerd naar het gewenste opslagaccount, volgt u de instructies in deze sectie voor het registreren van de VHD als een installatiekopie van het besturingssysteem of de schijf met besturingssysteem afhankelijk van uw scenario en maak een VM-exemplaar uit. De VHD met gegevensschijf kan worden gekoppeld aan de virtuele machine nadat deze is gemaakt.
Een voorbeeldscript van de migratie wordt verstrekt aan het einde van deze sectie. Dit eenvoudige script komt niet overeen met alle scenario's. Mogelijk moet u bijwerken van het script moet overeenkomen met uw specifieke scenario. Als dit script wordt toegepast op uw scenario, Zie onderstaande [een steekproef-Migratiescript](#a-sample-migration-script).

### <a name="checklist"></a>Controlelijst
1. Wacht totdat alle van de VHD-schijven kopiëren is voltooid.
2. Zorg ervoor dat Premium Storage is beschikbaar in de regio die u naar migreert.
3. Bepaal de nieuwe VM-serie die u wilt gebruiken. Dit moet een Premium-opslag kan worden, en de grootte moet zijn afhankelijk van de beschikbaarheid in de regio en op basis van uw behoeften.
4. Bepaal de exacte VM-grootte die u wilt gebruiken. VM-grootte moet groot genoeg is voor het aantal gegevensschijven hebt ondersteunen. Bijvoorbeeld Als u 4 gegevensschijven hebt, moet de virtuele machine 2 of meer kernen hebben. Overweeg ook verwerkingskracht, geheugen en netwerkbandbreedte nodig heeft.
5. Een Premium Storage-account maken in de doelregio. Dit is het account dat u voor de nieuwe virtuele machine gebruiken wilt.
6. De huidige details op de virtuele machine bij de hand hebt, met inbegrip van de lijst van schijven en de bijbehorende VHD-blobs zijn.

Uw toepassing voorbereiden voor uitvaltijd. U hebt hiervoor een schone migratie stoppen van de verwerking in het huidige systeem. Alleen dan kunt u deze ophalen naar consistente staat die u naar het nieuwe platform migreren kunt. Duur van uitvaltijd is afhankelijk van de hoeveelheid gegevens in de schijven om te migreren.

> [!NOTE]
> Als u een Azure Resource Manager-VM vanaf een gespecialiseerde VHD-schijf maakt, raadpleegt u [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) voor het implementeren van Resource Manager-VM met behulp van bestaande schijf.
>
>

### <a name="register-your-vhd"></a>Registreren van uw VHD
Om een virtuele machine van de VHD met besturingssysteem of een gegevensschijf koppelen aan een nieuwe virtuele machine, moet u deze eerst registreren. Volg onderstaande stappen, afhankelijk van uw VHD's scenario.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Besturingssysteem-VHD voor het maken van meerdere exemplaren van de Azure-VM gegeneraliseerd
Na het gegeneraliseerde installatiekopie van het besturingssysteem VHD uploaden naar het opslagaccount dat registreren als een **Azure VM-installatiekopie** zodat u een of meer VM-exemplaren van deze maken kunt. Gebruik de volgende PowerShell-cmdlets voor het registreren van uw VHD als een Azure VM-installatiekopie. Geef de volledige container-URL waarnaar de VHD is gekopieerd.

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

Kopieer en bewaar de naam van deze nieuwe Azure-VM-installatiekopie. In het bovenstaande voorbeeld is het *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>De unieke besturingssysteem-VHD voor het maken van één Azure VM-instantie
Nadat de unieke VHD met het besturingssysteem naar het opslagaccount dat is geüpload, registreert u dit als een **Azure Besturingssysteemschijf** zodat u een VM-exemplaar van deze maken kunt. Gebruik deze PowerShell-cmdlets voor het registreren van uw VHD als de schijf van een Azure-besturingssysteem. Geef de volledige container-URL waarnaar de VHD is gekopieerd.

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

Kopieer en bewaar de naam van deze nieuwe Azure-OS-schijf. In het bovenstaande voorbeeld is het *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Gegevensschijf moet worden gekoppeld aan de nieuwe Azure-VM-instantie (s)
Nadat de VHD met gegevensschijf is geüpload naar storage-account, registreert u dit als een Azure-gegevensschijf, zodat deze kan worden gekoppeld aan uw nieuwe DS-serie, DSv2-reeks of GS-serie Azure VM-exemplaar.

Gebruik deze PowerShell-cmdlets voor het registreren van uw VHD als een Azure-gegevensschijf. Geef de volledige container-URL waarnaar de VHD is gekopieerd.

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

Kopieer en bewaar de naam van deze nieuwe Azure-gegevensschijf. In het bovenstaande voorbeeld is het *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Een virtuele machine voor de staat van Premium-opslag maken
Nadat de installatiekopie van het besturingssysteem of de schijf met besturingssysteem zijn geregistreerd, maakt u een nieuwe DS-serie, DSv2-reeks of GS-serie VM. U gebruikt de installatiekopie van besturingssysteem of de naam van een besturingssysteem schijf die u hebt geregistreerd. Selecteer het type virtuele machine in de Premium Storage-laag. In onderstaand voorbeeld gebruiken we de *Standard_DS2* VM-grootte.

> [!NOTE]
> De grootte van de schijf om ervoor te zorgen dat deze overeenkomt met uw capaciteit en prestatie-eisen en grootte van de beschikbare Azure-schijven worden bijgewerkt.
>
>

Volg de stapsgewijze onderstaande PowerShell-cmdlets om de nieuwe VM te maken. Stel eerst de algemene parameters:

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

Maak eerst een cloudservice waarin u uw nieuwe virtuele machines worden gehost.

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

Maak vervolgens het Azure-VM-exemplaar van de installatiekopie van het besturingssysteem of de OS-schijf die u hebt geregistreerd, afhankelijk van uw scenario.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Besturingssysteem-VHD voor het maken van meerdere exemplaren van de Azure-VM gegeneraliseerd
Maken van een of meer DS-serie Azure VM-exemplaren met behulp van de **installatiekopie van het besturingssysteem Azure** die u hebt geregistreerd. Geef de naam van deze installatiekopie van het besturingssysteem in de VM-configuratie bij het maken van nieuwe virtuele machine, zoals hieronder wordt weergegeven.

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>De unieke besturingssysteem-VHD voor het maken van één Azure VM-instantie
Maak een nieuwe DS serie virtuele Azure-machine-exemplaar met de **Azure Besturingssysteemschijf** die u hebt geregistreerd. Geef de naam van dit OS-schijf in de VM-configuratie bij het maken van de nieuwe virtuele machine, zoals hieronder wordt weergegeven.

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

Geef andere Azure-VM-informatie, zoals een service in de cloud, regio, opslagaccount, beschikbaarheidsset en cachebeleid. Houd er rekening mee dat het VM-exemplaar geplaatst met de bijbehorende besturingssysteem of de gegevensschijven, worden moet zodat de geselecteerde cloud service, regio en storage-account op dezelfde locatie als de onderliggende virtuele harde schijven van deze schijven zijn moet.

### <a name="attach-data-disk"></a>Een gegevensschijf koppelen
Ten slotte, als u de gegevensschijf VHD's hebt geregistreerd, koppelt u ze naar de nieuwe Premium-opslag kan Azure VM.

Gebruikt u de volgende PowerShell-cmdlet gegevensschijf koppelen aan de nieuwe virtuele machine en het cachebeleid opgeven. In het volgende voorbeeld het cachebeleid is ingesteld op *ReadOnly*.

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> Mogelijk zijn er extra stappen die nodig is voor de ondersteuning van uw toepassing die niet worden behandeld in deze handleiding.
>
>

### <a name="checking-and-plan-backup"></a>Controleren en back-up plannen
Zodra de nieuwe virtuele machine actief en werkend is, toegang met behulp van de dezelfde aanmeldings-id en wachtwoord is als de oorspronkelijke virtuele machine en controleer of dat alles werkt zoals verwacht. Alle instellingen, met inbegrip van de striped volumes, zouden worden gebruikt in de nieuwe virtuele machine.

De laatste stap is het plannen van back-up en onderhoud plannen voor de nieuwe virtuele machine op basis van de behoeften van de toepassing.

### <a name="a-sample-migration-script"></a>Een voorbeeld van migratie van een script
Als u meerdere virtuele machines hebt te migreren, wordt de automatisering met behulp van PowerShell-scripts nuttig zijn. Hieronder volgt een voorbeeld van een script waarmee de migratie van een virtuele machine worden geautomatiseerd. Houd er rekening mee dat het onderstaande script is slechts een voorbeeld en er zijn enkele veronderstellingen over de huidige VM-schijven. Mogelijk moet u bijwerken van het script moet overeenkomen met uw specifieke scenario.

De veronderstellingen zijn:

* Klassieke Azure-VM's die u maakt.
* De bron-OS-schijven en de gegevensschijven van de bron zich in hetzelfde opslagaccount en dezelfde container. Als uw Besturingssysteemschijven en gegevensschijven niet op dezelfde plaats zijn, kunt u AzCopy of Azure PowerShell gebruiken om te kopiëren van VHD's over de storage-accounts en -containers. Raadpleeg de vorige stap: [Kopieer de VHD met het AzCopy- of PowerShell](#copy-vhd-with-azcopy-or-powershell). Bewerken van dit script om te voldoen aan uw scenario is een andere keuze, maar wij raden omdat het is eenvoudiger en sneller met behulp van de AzCopy- of PowerShell.

Hieronder vindt u het automatiseringsscript. Tekst vervangen door uw gegevens en bijwerken van het script moet overeenkomen met uw specifieke scenario.

> [!NOTE]
> Met behulp van het bestaande script behoudt niet de netwerkconfiguratie van de bron-VM. U moet re-config de netwerkinstellingen op de gemigreerde virtuele machines.
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <a name="optimization"></a>Optimalisatie
De huidige configuratie van de virtuele machine kan worden aangepast speciaal om te werken goed met standaardschijven. Bijvoorbeeld, de prestaties verbeteren door het gebruik van veel schijven in een striped volumes. Bijvoorbeeld, in plaats van 4 schijven afzonderlijk op Premium-opslag, u mogelijk de kosten optimaliseren door één schijf. Optimalisaties als deze niet hoeft te worden verwerkt op een per geval beoordeeld en aangepaste stappen moeten worden uitgevoerd na de migratie. Merk ook op dat dit proces werkt mogelijk niet goed voor databases en toepassingen die afhankelijk van de schijfindeling gedefinieerd in de instellingen zijn.

##### <a name="preparation"></a>Voorbereiding
1. De eenvoudige migratie voltooid zoals beschreven in de vorige sectie. Optimalisaties zal worden uitgevoerd op de nieuwe virtuele machine na de migratie.
2. De nieuwe schijfgrootte die nodig zijn voor de geoptimaliseerde configuratie definiëren.
3. Bepaal de toewijzing van de huidige schijven/volumes in de nieuwe schijf-specificaties.

##### <a name="execution-steps"></a>De stappen worden uitgevoerd
1. De nieuwe schijven maken met de juiste grootte beschikbaar op de VM voor Premium-opslag.
2. Meld u aan bij de virtuele machine en kopieer de gegevens van het huidige volume naar de nieuwe schijf die is toegewezen aan dat volume. Doe dit voor de huidige volumes die moeten worden toegewezen aan een nieuwe schijf.
3. Vervolgens de toepassingsinstellingen overschakelen naar de nieuwe schijven wijzigen en de oude volumes loskoppelen.

Raadpleeg voor de toepassing voor betere prestaties van de schijf afstemmen, [optimaliseert de prestaties van toepassingen](../../virtual-machines/windows/premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migraties van toepassing
Databases en andere complexe toepassingen kunnen speciale stappen vereist zoals gedefinieerd door de provider van de toepassing voor de migratie. Raadpleeg de documentatie van de desbetreffende toepassing. Bijvoorbeeld Normaal gesproken databases kunnen worden gemigreerd via back-up en herstellen.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor specifieke scenario's voor migratie van virtuele machines:

* [Azure virtuele Machines migreren tussen Opslagaccounts](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Maken en een Windows Server-VHD uploaden naar Azure.](../../virtual-machines/windows/upload-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Het maken en een Linux VHD uploaden naar Azure](../../virtual-machines/linux/create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Migreren van virtuele Machines van Amazon AWS naar Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zie ook de volgende bronnen voor meer informatie over Azure Storage en Azure Virtual Machines:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Premium Storage: Opslag met hoge prestaties voor workload in Azure Virtual Machine](../../virtual-machines/windows/premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
