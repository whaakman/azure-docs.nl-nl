---
title: OpenFOAM uitvoeren met HPC Pack op virtuele Linux-machines | Microsoft Docs
description: Een Microsoft HPC Pack-cluster in Azure implementeren en uitvoeren van een taak OpenFOAM op meerdere rekenknooppunten in Linux via een RDMA-netwerk.
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: 9032a0b68c4c8789010b0304b64a63d4924521fb
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "42058371"
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>OpenFoam uitvoeren met Microsoft HPC Pack op een Linux RDMA-cluster in Azure
Dit artikel ziet u een manier om OpenFoam uitvoeren in virtuele machines van Azure. Hier kunt u een Microsoft HPC Pack-cluster met Linux-rekenknooppunten in Azure en voer implementeert een [OpenFoam](http://openfoam.com/) taak met Intel MPI. Zodat de compute-knooppunten via het netwerk van Azure RDMA communiceren, kunt u RDMA-compatibele Azure-VM's voor de compute-knooppunten. Andere opties OpenFoam uitvoeren in Azure volledig geconfigureerde commerciële installatiekopieën die beschikbaar zijn in de Marketplace, zoals de UberCloud [OpenFoam 2.3 op CentOS 6](https://azuremarketplace.microsoft.com/marketplace/apps/cfd-direct.cfd-direct-from-the-cloud), en door te voeren op [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (voor veld bewerking openen en bewerken) is een open-source berekening van vloeistofdynamica (CFD) software-updatepakket dat veel in engineering en wetenschap, in commerciële en academische organisaties wordt gebruikt. Deze bevat hulpprogramma's voor meshing, met name snappyHexMesh, een geparallelliseerde mesher voor complexe CAD-geometrie, en voor vóór en na verwerking. Bijna alle processen worden uitgevoerd in parallelle, zodat gebruikers kunnen profiteren van de computerhardware beschikken.  

Microsoft HPC Pack biedt functies voor het uitvoeren van grootschalige HPC en parallelle toepassingen, waaronder MPI-toepassingen, op clusters van Microsoft Azure virtuele machines. HPC Pack biedt ook ondersteuning voor actieve Linux HPC-toepassingen op Linux-rekenknooppunt virtuele machines in een HPC Pack-cluster geïmplementeerd. Zie [aan de slag met Linux-rekenknooppunten in een HPC Pack-cluster in Azure](hpcpack-cluster.md) voor een inleiding tot het gebruik van Linux-rekenknooppunten met HPC Pack.

> [!NOTE]
> In dit artikel ziet u hoe u een Linux MPI-workload uitvoeren met HPC Pack. Wordt ervan uitgegaan dat u bekend zijn met Linux-Systeembeheer en bij het uitvoeren van MPI-workloads op Linux-clusters hebben. Als u versies van de MPI en OpenFOAM afwijken van de namen weergegeven in dit artikel gebruiken, hebt u mogelijk enkele stappen installatie en configuratie wijzigen. 
> 
> 

## <a name="prerequisites"></a>Vereisten
* **HPC Pack-cluster met RDMA-compatibele Linux-rekenknooppunten** : een HPC Pack-cluster met grootte A8, A9, H16r, implementeren of H16rm Linux-rekenknooppunten met behulp van een [Azure Resource Manager-sjabloon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](hpcpack-cluster-powershell-script.md). Zie [aan de slag met Linux-rekenknooppunten in een HPC Pack-cluster in Azure](hpcpack-cluster.md) voor de vereisten en stappen voor beide opties. Als u de PowerShell-script-implementatie-optie kiest, ziet u de voorbeeld-configuratiebestand in de voorbeeldbestanden aan het einde van dit artikel. Deze configuratie gebruiken voor het implementeren van een HPC Pack op basis van een Azure-cluster dat bestaat uit een grootte A8 Windows Server 2012 R2-hoofdknooppunt en 2 grootte A8 SUSE Linux Enterprise Server 12 compute-knooppunten. Vervangen door de juiste waarden voor uw abonnement en de service namen. 
  
  **Als u meer dingen die u moet weten**
  
  * Zie voor Linux RDMA-netwerken vereisten in Azure, [High performance computing-VM-grootten](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Als u de Powershell-script-implementatie-optie gebruikt, moet u alle Linux-rekenknooppunten binnen een cloudservice voor het gebruik van de RDMA-netwerkverbinding implementeren.
  * Na de implementatie van de Linux-knooppunten, verbinding maken met SSH naar eventuele extra administratieve taken uitvoeren. De details van de SSH-verbinding voor elke Linux VM in Azure portal vinden.  
* **Intel MPI** - OpenFOAM uitvoeren op SLES 12 HPC-rekenknooppunten in Azure, moet u voor het installeren van de runtime Intel MPI-bibliotheek 5 van de [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 is voorgeïnstalleerd op CentOS gebaseerde HPC-installatiekopieën.)  Installeer Intel MPI op uw Linux-rekenknooppunten in een latere stap, indien nodig. Om voor te bereiden voor deze stap nadat u zich bij Intel registreren, volgt u de koppeling in de bevestigingse-mail naar de gerelateerde webpagina. Kopieer vervolgens de downloadkoppeling voor het bestand .tgz voor de juiste versie van Intel MPI. In dit artikel is gebaseerd op Intel MPI versie 5.0.3.048.
* **OpenFOAM bron Pack** -Download de OpenFOAM bron Pack-software voor Linux uit de [OpenFOAM Foundation-site](http://openfoam.org/download/2-3-1-source/). In dit artikel is gebaseerd op bron Pack versie 2.3.1, gedownload als OpenFOAM 2.3.1.tgz. Volg de instructies verderop in dit artikel uitpakken en compileren OpenFOAM op de Linux-rekenknooppunten.
* **EnSight** (optioneel): de resultaten van uw OpenFOAM simulatie, downloadt en installeert u de [EnSight](https://ensighttransfe.wpengine.com/direct-access-downloads/) visualisatie en analyse-programma. Informatie over licentieverlening en download zijn op de site EnSight.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Wederzijdse vertrouwensrelatie tussen rekenknooppunten instellen
Een taak meerdere knooppunten wordt uitgevoerd op meerdere Linux-knooppunten vereist dat de knooppunten die elkaar vertrouwen (door **rsh** of **ssh**). Wanneer u het Microsoft HPC Pack IaaS-implementatiescript HPC Pack-cluster maakt, wordt het script automatisch stelt u permanente wederzijds vertrouwen voor het beheerdersaccount dat u opgeeft. Voor gebruikers die geen beheerder u in domein van het cluster maakt, kunt u hebt in te stellen van tijdelijke wederzijdse vertrouwensrelatie tussen de knooppunten wanneer een taak is toegewezen aan deze, en de relatie vernietigen nadat de taak voltooid is. Geef een RSA-sleutelpaar aan het cluster met HPC Pack voor de vertrouwensrelatie voor het maken van een vertrouwensrelatie voor elke gebruiker.

### <a name="generate-an-rsa-key-pair"></a>Een RSA-sleutelpaar niet genereren
Het is eenvoudig voor het genereren van een RSA-sleutelpaar, die een openbare sleutel en een persoonlijke sleutel bevat door het uitvoeren van de Linux **ssh-keygen** opdracht.

1. Meld u op een Linux-computer.
2. Voer de volgende opdracht uit:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Druk op **Enter** de standaardinstellingen gebruiken totdat de opdracht is voltooid. Voer een wachtwoordzin hier; niet Wanneer u hierom wordt gevraagd om een wachtwoord op te geven, drukt u op **Enter**.
   > 
   > 
   
   ![Een RSA-sleutelpaar niet genereren][keygen]
3. Wijzig de map naar de map ~/.ssh. De persoonlijke sleutel wordt opgeslagen in id_rsa en de openbare sleutel in id_rsa.pub.
   
   ![Persoonlijke en openbare sleutel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Het sleutelpaar toevoegen aan het cluster met HPC Pack
1. Een extern bureaublad verbinding maken met het hoofdknooppunt met uw beheerdersaccount HPC Pack (het administrator-account u hebt ingesteld toen u het script voor implementatie is uitgevoerd).
2. Standaardprocedures van Windows Server gebruiken om te maken van een domeingebruikersaccount in Active Directory-domein van het cluster. Bijvoorbeeld, gebruik het hulpprogramma Active Directory-gebruikers en Computers op het hoofdknooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een domeingebruiker die met de naam hpclab\hpcuser maken.
3. Maak een bestand met de naam C:\cred.xml en kopieer de gegevens van RSA-sleutel erin. Er is een voorbeeldbestand cred.xml aan het einde van dit artikel.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Open een opdrachtprompt en voer de volgende opdracht uit om de gegevens van de referenties voor de hpclab\hpcuser-account. U gebruikt de **extendeddata** parameter om door te geven van de naam van C:\cred.xml bestand dat u hebt gemaakt voor de belangrijkste gegevens.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Met deze opdracht is voltooid zonder uitvoer. Na het instellen van de referenties voor de gebruikersaccounts die u wilt uitvoeren van taken, de cred.xml-bestand op een veilige locatie opslaan of verwijderen.
5. Als u de RSA-sleutelpaar gegenereerd op een van uw Linux-knooppunten, moet u de sleutels verwijderen nadat u klaar bent met het gebruik ervan. Als HPC Pack vindt een bestaand id_rsa bestand of id_rsa.pub bestand, wordt deze niet wederzijds vertrouwen ingesteld.

> [!IMPORTANT]
> Wordt niet aanbevolen een Linux-taak als de Clusterbeheerder van een wordt uitgevoerd op een gedeelde cluster, omdat een taak die is verzonden door een beheerder wordt uitgevoerd onder het hoofdaccount op de Linux-knooppunten. Een taak die is verzonden door de gebruiker van een niet-beheerder wordt echter uitgevoerd onder een lokale Linux-gebruikersaccount met dezelfde naam als de gebruiker van de taak. In dit geval stelt HPC Pack u de vertrouwensrelatie voor deze gebruiker Linux op de knooppunten die zijn toegewezen aan de taak. U kunt de Linux-gebruiker handmatig op de Linux-knooppunten instellen voordat de taak wordt uitgevoerd of HPC Pack de gebruiker automatisch gemaakt wanneer de taak is verzonden. Als de gebruiker heeft gemaakt HPC Pack, HPC Pack wordt deze verwijderd nadat de taak is voltooid. Als u wilt verkleinen beveiligingsrisico's, verwijdert HPC Pack de sleutels nadat de taak is voltooid.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Instellen van een bestandsshare voor Linux-knooppunten
Nu instellen op een standaard SMB-share op een map op het hoofdknooppunt. Als u wilt toestaan dat de Linux-knooppunten te krijgen tot toepassingsbestanden met een algemene pad, de gedeelde map op de Linux-knooppunten te koppelen. Als u wilt, kunt u een andere optie, zoals een Azure Files-share - aanbevolen voor veel scenario's- of een NFS-share voor bestandsdeling. Zie het delen van informatie en gedetailleerde stappen in bestand [aan de slag met Linux-rekenknooppunten in een HPC Pack-Cluster in Azure](hpcpack-cluster.md).

1. Maak een map op het hoofdknooppunt en voor iedereen delen door het instellen van machtigingen voor lezen/schrijven. Bijvoorbeeld C:\OpenFOAM delen op het hoofdknooppunt als \\ \\SUSE12RDMA HN\OpenFOAM. Hier *SUSE12RDMA-HN* de hostnaam van het hoofdknooppunt.
2. Open een Windows PowerShell-venster en voer de volgende opdrachten uit:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

De eerste opdracht maakt een map met de naam /openfoam op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de //SUSE12RDMA-HN/OpenFOAM gedeelde map op de Linux-knooppunten met dir_mode en file_mode bits ingesteld op 777. De *gebruikersnaam* en *wachtwoord* in de opdracht moet de referenties van een gebruiker op het hoofdknooppunt.

> [!NOTE]
> De "\`" symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de ',' (komma) is een onderdeel van de opdracht.
> 
> 

## <a name="install-mpi-and-openfoam"></a>MPI en OpenFOAM installeren
Als u wilt OpenFOAM uitvoeren als een MPI-taak in de RDMA-netwerk, moet u OpenFOAM compileren met de Intel MPI-bibliotheken. 

Voer eerst uit verschillende **clusrun** opdrachten voor het installeren van Intel MPI-bibliotheken (indien nog niet is geïnstalleerd) en OpenFOAM op uw Linux-knooppunten. Het hoofdknooppunt-share die eerder zijn geconfigureerd voor het delen van de installatiebestanden van de Linux-knooppunten gebruiken.

> [!IMPORTANT]
> Deze installatie en compileren stappen zijn voorbeelden. Moet u enige kennis van beheer van de Linux-systeem om ervoor te zorgen dat afhankelijke compilers en bibliotheken correct zijn geïnstalleerd. U moet mogelijk bepaalde omgevingsvariabelen of andere instellingen voor de versies van Intel MPI en OpenFOAM aanpassen. Zie voor meer informatie, [Intel MPI-bibliotheek voor Linux-installatiehandleiding](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) en [OpenFOAM bron-Packinstallatie](http://openfoam.org/download/2-3-1-source/) voor uw omgeving.
> 
> 

### <a name="install-intel-mpi"></a>Intel MPI installeren
Sla het gedownloade installatiepakket voor Intel MPI (l_mpi_p_5.0.3.048.tgz in dit voorbeeld) in C:\OpenFoam op het hoofdknooppunt zodat de Linux-knooppunten toegang hebben tot dit bestand uit /openfoam. Voer **clusrun** Intel MPI-bibliotheek installeren op alle Linux-knooppunten.

1. De volgende opdrachten kopieert het installatiepakket en pak het /opt/intel op elk knooppunt.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. Als u wilt installeren op de achtergrond Intel MPI-bibliotheek, een silent.cfg-bestand te gebruiken. In de voorbeeldbestanden aan het einde van dit artikel vindt u een voorbeeld. Dit bestand in de gedeelde map /openfoam plaatsen. Zie voor meer informatie over het bestand silent.cfg [Intel MPI-bibliotheek voor de installatiehandleiding voor Linux - installatie op de achtergrond](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Zorg ervoor dat u uw silent.cfg bestand opslaan als een tekstbestand met Linux regeleinden heb (alleen LF, niet CR LF). Deze stap zorgt ervoor dat deze correct kan worden uitgevoerd op de Linux-knooppunten.
   > 
   > 
3. Intel MPI-bibliotheek in stille modus installeren.
   
   ```
   clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
   ```

### <a name="configure-mpi"></a>MPI configureren
Voor het testen, moet u de volgende regels toevoegen aan de /etc/security/limits.conf op elk van de Linux-knooppunten:

    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Start opnieuw op de Linux-knooppunten na het bijwerken van het bestand limits.conf. Bijvoorbeeld, gebruikt u de volgende **clusrun** opdracht:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Zorg ervoor dat de gedeelde map is gekoppeld als /openfoam nadat de computer opnieuw is opgestart.

### <a name="compile-and-install-openfoam"></a>Compileren en OpenFOAM installeren
Sla het gedownloade installatiepakket voor het OpenFOAM bron Pack (OpenFOAM-2.3.1.tgz in dit voorbeeld) voor C:\OpenFoam op het hoofdknooppunt zodat de Linux-knooppunten toegang hebben tot dit bestand uit /openfoam. Voer **clusrun** opdrachten voor het compileren van OpenFOAM op alle Linux-knooppunten.

1. Maken van een map /opt/OpenFOAM op elk knooppunt Linux, de bronpakket kopiëren naar deze map en pak het er uit.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. OpenFOAM met de Intel MPI-bibliotheek eerst enkele omgevingsvariabelen instellen voor Intel MPI en OpenFOAM compileren. Gebruik een bash-script met de naam settings.sh om in te stellen van de variabelen. In de voorbeeldbestanden aan het einde van dit artikel vindt u een voorbeeld. Dit bestand (opgeslagen met regeleinden Linux) in de gedeelde map /openfoam plaatsen. Dit bestand bevat ook de instellingen voor de MPI en OpenFOAM runtimes dat u later gebruiken een OpenFOAM-taak uit te voeren.
3. Afhankelijke pakketten die nodig zijn voor het compileren van OpenFOAM installeren. Afhankelijk van de Linux-distributie moet u mogelijk eerst een opslagplaats toevoegen. Voer **clusrun** opdrachten is vergelijkbaar met het volgende:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Indien nodig, SSH op elk knooppunt Linux om uit te voeren van de opdrachten om te bevestigen dat ze naar behoren uitgevoerd.
4. De volgende opdracht voor het compileren van OpenFOAM uitvoeren. De compilatie-proces neemt enige tijd in beslag en genereert een grote hoeveelheid gegevens naar de standaarduitvoer, Gebruik daarom de **/ interleaved** optie om de uitvoer interleaved weer te geven.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > De "\`" symbool in de opdracht is een escapeteken voor PowerShell. "\`&" betekent de "&" is een onderdeel van de opdracht.
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a>Voorbereiden op een taak OpenFOAM uitvoeren
Nu aan de slag met een MPI-taak met de naam sloshingTank3D, dit een van de voorbeelden OpenFoam is, op twee Linux-knooppunten worden uitgevoerd. 

### <a name="set-up-the-runtime-environment"></a>De runtime-omgeving instellen
Als u de runtime-omgevingen voor MPI- en OpenFOAM instelt op de Linux-knooppunten, moet u de volgende opdracht uitvoeren in een Windows PowerShell-venster op het hoofdknooppunt. (Deze opdracht is alleen geldig voor SUSE Linux.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden
Het hoofdknooppunt-share die u eerder hebt geconfigureerd voor het delen van bestanden tussen de Linux-knooppunten (gekoppeld als /openfoam) gebruiken.

1. SSH naar een van uw Linux-rekenknooppunten.
2. Voer de volgende opdracht voor het instellen van de OpenFOAM runtime-omgeving als u dit nog niet hebt gedaan.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Het voorbeeld sloshingTank3D kopiëren naar de gedeelde map en Ga naar deze.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Wanneer u de standaardparameters van dit voorbeeld gebruikt, duurt het tien minuten uit te voeren, zodat u wijzigen van bepaalde parameters wilt mogelijk zodat deze sneller worden uitgevoerd. Een eenvoudige keuze is om de tijd stap variabelen deltaT en writeInterval in het bestand system/controlDict te wijzigen. Dit bestand slaat alle ingevoerde gegevens met betrekking tot het beheer van de tijd- en lezen en schrijven van oplossingsgegevens. U kunt bijvoorbeeld de waarde van deltaT van 0,05 op 0,5 en de waarde van writeInterval van 0,05 op 0,5 wijzigen.
   
   ![Wijzig de variabelen in stap][step_variables]
5. Geef de gewenste waarden voor de variabelen in het bestand system/decomposeParDict. In dit voorbeeld maakt gebruik van twee knooppunten in Linux elke met 8 kernen, dus numberOfSubdomains ingesteld op 16 en n van hierarchicalCoeffs aan (1 1 16), wat betekent dat OpenFOAM parallel met 16 processen worden uitgevoerd. Zie voor meer informatie, [OpenFOAM User Guide: 3.4 uitgevoerd toepassingen parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![Processen afbreken][decompose]
6. Voer de volgende opdrachten uit de map sloshingTank3D het voorbereiden van de voorbeeldgegevens.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. Op het hoofdknooppunt ziet u dat de bestanden met voorbeeldgegevens zijn gekopieerd naar C:\OpenFoam\sloshingTank3D. (C:\OpenFoam is de gedeelde map op het hoofdknooppunt.)
   
   ![Gegevensbestanden op het hoofdknooppunt][data_files]

### <a name="host-file-for-mpirun"></a>Hostbestand voor mpirun
In deze stap maakt u een hostbestand (een lijst met compute-knooppunten) die de **mpirun** gebruikt de opdracht.

1. Maak op een van de Linux-knooppunten, een bestand met de naam hostfile onder /openfoam, zodat dit bestand kan worden bereikt op /openfoam/hostfile op alle Linux-knooppunten.
2. De namen van uw Linux-knooppunt naar dit bestand schrijven. In dit voorbeeld bevat het bestand de volgende namen:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > U kunt dit bestand ook op C:\OpenFoam\hostfile maken op het hoofdknooppunt. Als u deze optie kiest, opslaan als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze correct kan worden uitgevoerd op de Linux-knooppunten.
   > 
   > 
   
   **Bash-script-wrapper**
   
   Als u veel Linux-knooppunten hebt en u wilt dat uw taak uit te voeren op slechts een deel ervan, is het niet een goed idee om een hostbestand vaste te gebruiken omdat u niet weet welke knooppunten wordt toegewezen aan uw taak. In dit geval een bash-script-wrapper voor schrijven **mpirun** bestand met de host om automatisch te maken. U kunt een voorbeeld van de bash-script wrapper met de naam hpcimpirun.sh aan het einde van dit artikel vinden en sla deze op als /openfoam/hpcimpirun.sh. Dit voorbeeldscript doet het volgende:
   
   1. Stelt u de omgevingsvariabelen voor **mpirun**, en sommige parameters van de opdracht toevoegen voor het uitvoeren van de taak MPI via het netwerk van RDMA. In dit geval wordt de volgende variabelen:
      
      * I_MPI_FABRICS shm:dapl =
      * I_MPI_DAPL_PROVIDER=ofa-v2-ib0
      * I_MPI_DYNAMIC_CONNECTION = 0
   2. Hiermee maakt u een hostbestand op basis van de omgeving variabele $CCP_NODES_CORES, die door de HPC-hoofdknooppunt wordt ingesteld wanneer de taak wordt geactiveerd.
      
      De indeling van $CCP_NODES_CORES volgt dit patroon:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      waar
      
      * `<Number of nodes>` -het aantal knooppunten dat is toegewezen aan deze taak.  
      * `<Name of node_n_...>` -de naam van elk knooppunt dat is toegewezen aan deze taak.
      * `<Cores of node_n_...>` -het aantal kernen op het knooppunt dat is toegewezen aan deze taak.
      
      Bijvoorbeeld, als de taak twee knooppunten om uit te voeren moet, is $CCP_NODES_CORES vergelijkbaar met
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Aanroepen van de **mpirun** opdracht en voegt u twee parameters toe aan de opdrachtregel.
      
      * `--hostfile <hostfilepath>: <hostfilepath>` -het pad van de hostbestand dat het script wordt gemaakt
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` -een omgevingsvariabele instellen met het hoofdknooppunt HPC Pack, waarbij het nummer van het totaal aantal kerngeheugens is toegewezen aan deze taak worden opgeslagen. In dit geval het Hiermee geeft u het aantal processen voor **mpirun**.

## <a name="submit-an-openfoam-job"></a>Een taak OpenFOAM verzenden
Nu kunt u een taak in HPC Cluster Manager verzenden. U moet het script hpcimpirun.sh doorgeven in de opdrachtregels voor enkele van de taken.

1. Verbinding maken met het hoofdknooppunt van het cluster en start HPC Cluster Manager.
2. **In resourcebeheer**, zorg ervoor dat de Linux-rekenknooppunten in de **Online** staat. Als dat niet het geval is, selecteert u deze en klikt u op **Online brengen**.
3. In **Taakbeheer**, klikt u op **nieuwe taak**.
4. Voer een naam voor de taak bijvoorbeeld *sloshingTank3D*.
   
   ![Taakdetails][job_details]
5. In **taak resources**, kies het type resource als 'Knooppunt' en de minimale ingesteld op 2. Deze configuratie voert de taak op twee knooppunten in Linux, elk met acht kernen in dit voorbeeld heeft.
   
   ![Taakresources][job_resources]
6. Klik op **taken bewerken** in de navigatiebalk aan de linkerkant en klik vervolgens op **toevoegen** een taak wordt toegevoegd aan de job. Vier taken toevoegen aan de taak met de volgende opdrachtregels en instellingen.
   
   > [!NOTE]
   > Met `source /openfoam/settings.sh` stelt u de OpenFOAM en MPI-runtime-omgevingen, zodat elk van de volgende taken vóór de opdracht OpenFOAM aangeroepen.
   > 
   > 
   
   * **Taak 1**. Voer **decomposePar** voor het genereren van de gegevensbestanden voor het werken met **interDyMFoam** parallel.
     
     * Toewijzen van een knooppunt aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
     
     Zie de volgende afbeelding. U configureren de resterende taken op dezelfde manier.
     
     ![Taak 1-details][task_details1]
   * **Taak 2**. Voer **interDyMFoam** parallel voor het berekenen van het voorbeeld.
     
     * Twee knooppunten toewijzen aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
   * **Taak 3**. Voer **reconstructPar** voor het samenvoegen van de sets tijd directory's van elke map processor_N_ in één set.
     
     * Toewijzen van een knooppunt aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
   * **Taak 4**. Voer **foamToEnsight** parallel te converteren van het resultaat OpenFOAM bestanden naar EnSight formatteren en de bestanden EnSight plaatsen in een map met de naam Ensight in de map met de aanvraag.
     
     * Twee knooppunten toewijzen aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
7. Afhankelijkheden toevoegen aan deze taken in oplopende taakvolgorde.
   
   ![Taakafhankelijkheden][task_dependencies]
8. Klik op **indienen** deze taak uit te voeren.
   
   Standaard verzendt HPC Pack de taak als uw huidige aangemelde gebruikersaccount gebruikt. Nadat u op **indienen**, ziet u mogelijk een dialoogvenster waarin u de gebruikersnaam en wachtwoord invoeren.
   
   ![Taakreferenties][creds]
   
   HPC Pack onthoudt onder bepaalde omstandigheden, de gebruikersgegevens die u voor de invoer en dit dialoogvenster niet weergeven. Voer de volgende opdracht achter de opdrachtprompt zodat HPC Pack opnieuw weergeven en vervolgens verzendt de taak.
   
   ```
   hpccred delcreds
   ```
9. De taak duurt van tientallen van minuten tot enkele uren op basis van de parameters die u hebt ingesteld voor het voorbeeld. In de heatmap ziet u de taak wordt uitgevoerd op de Linux-knooppunten. 
   
   ![Heatmap][heat_map]
   
   Op elk knooppunt zijn acht processen gestart.
   
   ![Linux-processen][linux_processes]
10. De taak is voltooid, vindt de resultaten van de taak in mappen onder C:\OpenFoam\sloshingTank3D en de logboekbestanden op C:\OpenFoam.

## <a name="view-results-in-ensight"></a>Resultaten weergeven in EnSight
(Optioneel) Gebruik [EnSight](http://www.ensight.com/) visualiseren en analyseren van de resultaten van de taak OpenFOAM. Zie voor meer informatie over de visualisatie en animatie in EnSight, [video handleiding](http://www.ensight.com/ensight.com/envideo/).

1. Nadat u op het hoofdknooppunt EnSight hebt geïnstalleerd, start u deze.
2. Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   U ziet in een in de viewer.
   
   ![Brandstoftank in EnSight][tank]
3. Maak een **Isosurface** van **internalMesh**, en kies vervolgens de variabele **alpha_water**.
   
   ![Een isosurface maken][isosurface]
4. Stel de kleur voor **Isosurface_part** in de vorige stap hebt gemaakt. Bijvoorbeeld, dit instellen op water blauw.
   
   ![Isosurface kleur bewerken][isosurface_color]
5. Maakt een **Iso-volume** van **wanden** door te selecteren **wanden** in de **delen** deelvenster en klik op de **Isosurfaces** knop op de werkbalk.
6. Selecteer in het dialoogvenster **Type** als **Isovolume** en stel de minimale waarde van **Isovolume bereik** op 0,5. Klik op om de isovolume **maken met de geselecteerde onderdelen**.
7. Stel de kleur voor **Iso_volume_part** in de vorige stap hebt gemaakt. Bijvoorbeeld, dit instellen op diepe water blauw.
8. Stel de kleur voor **wanden**. Bijvoorbeeld, dit instellen op transparante wit.
9. Klik nu op **afspelen** om de resultaten van de simulatie te bekijken.
   
    ![Brandstoftank resultaat][tank_result]

## <a name="sample-files"></a>Voorbeeldbestanden
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Voorbeeld van XML-configuratiebestand voor implementatie van het cluster door PowerShell-script
 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Voorbeeldbestand cred.xml
```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Silent.cfg-voorbeeldbestand voor het installeren van de MPI
```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Voorbeeld van een script settings.sh
```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


### <a name="sample-hpcimpirunsh-script"></a>Voorbeeld van een script hpcimpirun.sh
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]:media/hpcpack-cluster-openfoam/keygen.png
[keys]:media/hpcpack-cluster-openfoam/keys.png
[step_variables]:media/hpcpack-cluster-openfoam/step_variables.png
[data_files]:media/hpcpack-cluster-openfoam/data_files.png
[decompose]:media/hpcpack-cluster-openfoam/decompose.png
[job_details]:media/hpcpack-cluster-openfoam/job_details.png
[job_resources]:media/hpcpack-cluster-openfoam/job_resources.png
[task_details1]:media/hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]:media/hpcpack-cluster-openfoam/task_dependencies.png
[creds]:media/hpcpack-cluster-openfoam/creds.png
[heat_map]:media/hpcpack-cluster-openfoam/heat_map.png
[tank]:media/hpcpack-cluster-openfoam/tank.png
[tank_result]:media/hpcpack-cluster-openfoam/tank_result.png
[isosurface]:media/hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]:media/hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]:media/hpcpack-cluster-openfoam/linux_processes.png
