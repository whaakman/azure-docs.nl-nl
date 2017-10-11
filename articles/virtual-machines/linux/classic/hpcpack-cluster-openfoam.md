---
title: OpenFOAM HPC Pack uitvoeren op virtuele Linux-machines | Microsoft Docs
description: Een Microsoft HPC Pack cluster in Azure implementeren en uitvoeren van een taak OpenFOAM op meerdere rekenknooppunten van Linux via een netwerk RDMA.
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: c0bb1637-bb19-48f1-adaa-491808d3441f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 07/22/2016
ms.author: danlep
ms.openlocfilehash: ef124a8983fa112d499252460bff9ed2fcccc02b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>OpenFoam uitvoeren met Microsoft HPC Pack op een Linux RDMA-cluster in Azure
In dit artikel laat zien hoe OpenFoam uitvoeren in virtuele machines in Azure. Hier kunt u een cluster met Microsoft HPC Pack met Linux-rekenknooppunten in Azure en voer implementeert een [OpenFoam](http://openfoam.com/) taak met de Intel MPI. Zodat de rekenknooppunten via het netwerk van Azure RDMA communiceren, kunt u RDMA-compatibele Azure VM's voor de rekenknooppunten. Andere opties OpenFoam uitvoeren in Azure, volledig geconfigureerde commerciële installatiekopieën beschikbaar zijn in de Marketplace, zoals de UberCloud [OpenFoam 2.3 op CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/), en door te voeren op [Azure Batch](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/). 

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (voor veld bewerking openen en bewerken) is een open source computational fluid dynamics (CFD)-softwarepakket dat algemeen wordt gebruikt in de technische en wetenschappelijke in commerciële en academic organisaties. Dit omvat de hulpprogramma's voor meshing, met name snappyHexMesh, een parallelized mesher voor complexe CAD-geometrie en voor vóór en na verwerking. Bijna alle processen parallel uitgevoerd, zodat gebruikers kunnen profiteren van de computerhardware beschikken.  

Microsoft HPC Pack beschikt over functies voor het uitvoeren van grootschalige HPC en parallelle toepassingen, waaronder MPI-toepassingen op clusters van Microsoft Azure virtuele machines. HPC Pack ondersteunt ook actief Linux HPC-toepassingen op Linux VM's geïmplementeerd in een cluster HPC Pack rekenknooppunt. Zie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md) voor een inleiding tot het gebruik van Linux rekenknooppunten HPC Pack.

> [!NOTE]
> In dit artikel laat zien hoe een Linux MPI-werkbelasting met HPC Pack uitvoeren. Wordt ervan uitgegaan dat u enigszins bekend bent met Linux Systeembeheer en MPI belastingen uitgevoerd op Linux-clusters hebben. Als u versies van MPI en OpenFOAM afwijken van de namen weergegeven in dit artikel gebruikt, moet u wellicht een aantal stappen installatie en configuratie wijzigen. 
> 
> 

## <a name="prerequisites"></a>Vereisten
* **HPC Pack cluster met RDMA-compatibele Linux-rekenknooppunten** : een cluster HPC Pack met een grootte A8, A9, H16r, implementeren of H16rm Linux-rekenknooppunten met behulp van een [Azure Resource Manager-sjabloon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](hpcpack-cluster-powershell-script.md). Zie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md) voor de vereisten en stappen voor beide opties. Als u de PowerShell-script-implementatie-optie kiest, raadpleegt u het voorbeeldconfiguratiebestand in de voorbeeldbestanden aan het einde van dit artikel. Deze configuratie gebruiken voor het implementeren van een HPC Pack op basis van een Azure-cluster die bestaan uit een grootte A8 Windows Server 2012 R2-hoofdknooppunt en 2 grootte A8 SUSE Linux Enterprise Server 12-rekenknooppunten. Vervang de juiste waarden voor uw abonnement en de service namen. 
  
  **Aanvullende zaken die u moet weten**
  
  * Zie voor Linux RDMA netwerken vereisten in Azure, [hoge prestaties compute-VM-grootten](../../windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
  * Als u de optie voor de Powershell-script gebruikt, implementeert u alle Linux-rekenknooppunten in een cloudservice te gebruiken van de RDMA-netwerkverbinding.
  * Na implementatie van de Linux-knooppunten, verbinding maken via SSH eventuele extra beheertaken uitvoeren. De details van de SSH-verbinding voor elke Linux-VM niet vinden in de Azure-portal.  
* **Intel MPI** - OpenFOAM uitvoeren op SLES 12 HPC-rekenknooppunten in Azure, moet u voor het installeren van de runtime Intel MPI-bibliotheek 5 van de [Intel.com site](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 is voorgeïnstalleerd op op basis van CentOS HPC-installatiekopieën.)  Installeer Intel MPI op uw Linux-rekenknooppunten in een latere stap, indien nodig. Om voor te bereiden voor deze stap nadat u hebt geregistreerd met Intel, volgt u de koppeling in het bevestigingsbericht naar de gerelateerde webpagina. Kopieer vervolgens de downloadkoppeling voor het bestand .tgz voor de juiste versie van Intel MPI. In dit artikel is gebaseerd op Intel MPI versie 5.0.3.048.
* **OpenFOAM bron Pack** -Download de OpenFOAM bron Pack-software voor Linux van de [OpenFOAM Foundation-site](http://openfoam.org/download/2-3-1-source/). In dit artikel is gebaseerd op de bron-packversie 2.3.1-specificaties gedownload als OpenFOAM 2.3.1.tgz. Volg de instructies verderop in dit artikel om te pakken en compileren OpenFOAM op de Linux-rekenknooppunten.
* **EnSight** (optioneel): de resultaten van uw OpenFOAM simulatie, downloadt en installeert de [EnSight](https://www.ceisoftware.com/download/) visualisatie en analyse-programma. Licentie-en download zijn op de site EnSight.

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Wederzijdse vertrouwensrelatie tussen de rekenknooppunten instellen
Een taak cross-knooppunt wordt uitgevoerd op meerdere Linux-knooppunten vereist dat de knooppunten die elkaar vertrouwen (door **rsh** of **ssh**). Wanneer u het cluster HPC Pack met het script voor Microsoft HPC Pack IaaS-implementatie maakt, het script permanente wederzijds vertrouwen voor de administrator-account dat u opgeeft automatisch ingesteld. Voor gebruikers die geen beheerder u in domein van het cluster maakt, hebben om in te stellen van tijdelijke wederzijds vertrouwen tussen de knooppunten wanneer een taak wordt toegewezen aan hen en de relatie vernietigen nadat de taak voltooid is. Als u wilt een vertrouwensrelatie voor elke gebruiker, bieden een RSA-sleutelpaar naar het cluster dat HPC Pack voor de vertrouwensrelatie gebruikt.

### <a name="generate-an-rsa-key-pair"></a>Een RSA-sleutelpaar niet genereren
Het is gemakkelijk voor het genereren van een RSA-sleutelpaar, die een openbare sleutel en een persoonlijke sleutel bevat door het uitvoeren van de Linux **ssh-keygen** opdracht.

1. Meld u op een Linux-computer.
2. Voer de volgende opdracht uit:
   
   ```
   ssh-keygen -t rsa
   ```
   
   > [!NOTE]
   > Druk op **Enter** de standaardinstellingen gebruiken totdat de opdracht is voltooid. Voer een wachtwoordzin hier; niet Wanneer u daarom wordt gevraagd om een wachtwoord op te geven, drukt u op **Enter**.
   > 
   > 
   
   ![Een RSA-sleutelpaar niet genereren][keygen]
3. Map naar de map ~/.ssh wijzigen. De persoonlijke sleutel wordt opgeslagen in id_rsa en de openbare sleutel in id_rsa.pub.
   
   ![Persoonlijke en openbare sleutel][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Het sleutelpaar aan het cluster HPC Pack toevoegen
1. Extern bureaublad verbinding maken met uw hoofdknooppunt met uw beheerdersaccount HPC Pack (het administrator-account u instellen wanneer u het script voor implementatie is uitgevoerd).
2. Gebruik standaard Windows Server-procedures om een domeingebruikersaccount maken in Active Directory-domein van het cluster. Bijvoorbeeld, gebruik het hulpprogramma Active Directory-gebruikers en Computers op het hoofdknooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een domeingebruiker die met de naam hpclab\hpcuser maken.
3. Maak een bestand met de naam C:\cred.xml en kopieer de RSA-sleutelgegevens in de App. Er is een voorbeeldbestand cred.xml aan het einde van dit artikel.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
4. Open een opdrachtprompt en voer de volgende opdracht om de gegevens van de referenties voor het account hpclab\hpcuser instellen. U gebruikt de **extendeddata** -parameter voor de naam van C:\cred.xml dat u hebt gemaakt voor de belangrijkste gegevens doorgeeft.
   
   ```
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Deze opdracht is voltooid zonder uitvoer. Na het instellen van de referenties voor de gebruikersaccounts die u wilt uitvoeren van taken, sla het bestand cred.xml in een veilige locatie of verwijder deze.
5. Als u de RSA-sleutelpaar gegenereerd op een van uw Linux-knooppunten, moet u de sleutels verwijderen nadat u klaar bent met het gebruik ervan. HPC Pack vindt een bestaande id_rsa bestand of id_rsa.pub-bestand, dan wordt deze wederzijds vertrouwen niet ingesteld.

> [!IMPORTANT]
> Wordt niet aanbevolen als de Clusterbeheerder van een een Linux-taak wordt uitgevoerd op een gedeelde cluster, omdat een taak die is verzonden door een beheerder wordt uitgevoerd onder het hoofdaccount op de Linux-knooppunten. Een taak die is verzonden door een gebruiker die geen beheerder wordt echter uitgevoerd onder een lokale Linux-gebruikersaccount met dezelfde naam als de gebruiker van de taak. In dit geval stelt HPC Pack wederzijds vertrouwen voor deze gebruiker Linux op de knooppunten die zijn toegewezen aan de taak. U kunt de Linux-gebruiker handmatig op de Linux-knooppunten instellen voordat de taak wordt uitgevoerd of HPC Pack de gebruiker wordt automatisch gemaakt wanneer de taak wordt verzonden. Als HPC Pack de gebruiker maakt, verwijderd HPC Pack nadat de taak is voltooid. Als u beveiligingsrisico's, verwijdert HPC Pack de sleutels na Taakvoltooiing van de.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Instellen van een bestandsshare voor Linux-knooppunten
Nu ingesteld op een standaard SMB-share op een map op het hoofdknooppunt. Koppel de gedeelde map op de Linux-knooppunten zodat de Linux-knooppunten toegang tot toepassingsbestanden met een algemeen pad. Als u wilt, kunt u een andere optie, zoals een Azure-bestanden share - aanbevolen voor veel scenario's- of een NFS-share voor bestandsdeling. Zie het bestand delen van informatie en gedetailleerde stappen in [aan de slag met Linux-rekenknooppunten in een HPC Pack-Cluster in Azure](hpcpack-cluster.md).

1. Maak een map op het hoofdknooppunt en voor iedereen delen door het instellen van machtigingen voor lezen/schrijven. Bijvoorbeeld C:\OpenFOAM delen op het hoofdknooppunt als \\ \\SUSE12RDMA HN\OpenFOAM. Hier *SUSE12RDMA HN* is de hostnaam van het hoofdknooppunt.
2. Open een Windows PowerShell-venster en voer de volgende opdrachten:
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /openfoam
   
   clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
   ```

De eerste opdracht maakt een map met de naam /openfoam op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de //SUSE12RDMA-HN/OpenFOAM gedeelde map op de Linux-knooppunten met dir_mode en file_mode bits ingesteld op 777. De *gebruikersnaam* en *wachtwoord* in de opdracht moet de referenties van een gebruiker op het hoofdknooppunt.

> [!NOTE]
> De '\`' symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de","(kommateken) maakt deel uit van de opdracht.
> 
> 

## <a name="install-mpi-and-openfoam"></a>MPI en OpenFOAM installeren
OpenFOAM als een MPI-taak op het netwerk RDMA, hebt u nodig voor het compileren van OpenFOAM met de Intel MPI-bibliotheken. 

Voer eerst enkele **clusrun** opdrachten voor het installeren van Intel MPI-bibliotheken (indien nog niet is geïnstalleerd) en OpenFOAM op uw Linux-knooppunten. Het hoofdknooppunt share eerder geconfigureerd voor het delen van de van installatiebestanden tussen de Linux-knooppunten gebruiken.

> [!IMPORTANT]
> Deze installatie en gecompileerd stappen zijn voorbeelden. Moet u enige kennis van de Linux-systeembeheertaken om ervoor te zorgen dat afhankelijke compilers en bibliotheken correct zijn geïnstalleerd. Mogelijk moet u bepaalde omgevingsvariabelen of andere instellingen voor uw versie van Intel MPI en OpenFOAM wijzigen. Zie voor meer informatie [Intel MPI-bibliotheek voor Linux-installatiehandleiding](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) en [OpenFOAM bron Pack installatie](http://openfoam.org/download/2-3-1-source/) voor uw omgeving.
> 
> 

### <a name="install-intel-mpi"></a>Intel MPI installeren
Sla het installatiepakket voor de gedownloade voor Intel MPI (l_mpi_p_5.0.3.048.tgz in dit voorbeeld) in C:\OpenFoam hoofdknooppunt van het zodat de Linux-knooppunten toegang dit bestand uit /openfoam tot. Voer **clusrun** Intel MPI-bibliotheek te installeren op alle Linux-knooppunten.

1. De volgende opdrachten Kopieer het installatiepakket en pak het /opt/intel op elk knooppunt.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
   ```
2. U kunt Intel MPI-bibliotheek achtergrond installeren met een silent.cfg-bestand. In de voorbeeldbestanden aan het einde van dit artikel vindt u een voorbeeld. Dit bestand in de gedeelde map /openfoam plaatsen. Zie voor meer informatie over het bestand silent.cfg [Intel MPI-bibliotheek voor de installatiehandleiding voor Linux - installatie op de achtergrond](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).
   
   > [!TIP]
   > Zorg ervoor dat u uw silent.cfg bestand opslaan als een tekstbestand met Linux regeleinden heb (alleen LF, niet CR LF). Deze stap zorgt ervoor dat deze goed op de Linux-knooppunten.
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


Na het bijwerken van het bestand limits.conf, start opnieuw op de Linux-knooppunten. Bijvoorbeeld, gebruikt u de volgende **clusrun** opdracht:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Start opnieuw op en controleer of de gedeelde map is gekoppeld als /openfoam.

### <a name="compile-and-install-openfoam"></a>Compileren en OpenFOAM installeren
Het installatiepakket voor de gedownloade voor OpenFOAM bron Pack (OpenFOAM-2.3.1.tgz in dit voorbeeld) naar C:\OpenFoam opslaan op het hoofdknooppunt zodat de Linux-knooppunten toegang dit bestand uit /openfoam tot. Voer **clusrun** opdrachten voor het compileren van OpenFOAM op alle Linux-knooppunten.

1. Een map /opt/OpenFOAM op elk knooppunt Linux maken, de bronpakket kopiëren naar deze map en er extraheren.
   
   ```
   clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM
   
   clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
   
   clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
   ```
2. Voor het compileren van OpenFOAM met de Intel MPI-bibliotheek eerst enkele omgevingsvariabelen instellen voor Intel MPI- en OpenFOAM. Een bash-script settings.sh aangeroepen om in te stellen van de variabelen gebruiken. In de voorbeeldbestanden aan het einde van dit artikel vindt u een voorbeeld. Dit bestand (opgeslagen met regeleinden Linux) in de gedeelde map /openfoam plaatsen. Dit bestand bevat ook de instellingen voor de MPI en OpenFOAM runtimes die u later gebruiken een OpenFOAM-taak uit te voeren.
3. Afhankelijke pakketten nodig voor het compileren van OpenFOAM installeren. Mogelijk moet u eerst het toevoegen van een opslagplaats, afhankelijk van uw Linux-distributie. Voer **clusrun** vergelijkbaar met de volgende opdrachten:
   
    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
   
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
   
    Indien nodig, SSH op elk knooppunt Linux uitvoeren van de opdrachten om te bevestigen dat ze naar behoren uitgevoerd.
4. Voer de volgende opdracht voor het compileren van OpenFOAM. De compilatie-proces neemt enige tijd in beslag en genereert een grote hoeveelheid informatie naar de standaarduitvoer in het foutenlogboek, gebruikt u dus de **/ interleaved** optie om de uitvoer interleaved weer te geven.
   
   ```
   clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
   ```
   
   > [!NOTE]
   > De '\`' symbool in de opdracht is een escapeteken voor PowerShell. "\`&" betekent de "&" deel uitmaakt van de opdracht.
   > 
   > 

## <a name="prepare-to-run-an-openfoam-job"></a>Voorbereiden van een taak OpenFOAM uitvoeren
Nu voorbereidingen voor het uitvoeren van een MPI-taak sloshingTank3D, namelijk een van de voorbeelden OpenFoam aangeroepen in twee Linux-knooppunten. 

### <a name="set-up-the-runtime-environment"></a>De runtime-omgeving instellen
Als u de runtimeomgevingen voor MPI en OpenFOAM instelt op de Linux-knooppunten, voer de volgende opdracht in een Windows PowerShell-venster op het hoofdknooppunt. (Deze opdracht is alleen geldig voor SUSE Linux.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Voorbeeldgegevens voorbereiden
Het hoofdknooppunt-share die u eerder hebt geconfigureerd voor het delen van bestanden tussen de Linux-knooppunten (gekoppeld als /openfoam) gebruiken.

1. SSH op een van uw Linux-rekenknooppunten.
2. Voer de volgende opdracht voor het instellen van de OpenFOAM runtime-omgeving, als u dit nog niet hebt gedaan.
   
   ```
   $ source /openfoam/settings.sh
   ```
3. Het voorbeeld sloshingTank3D kopiëren naar de gedeelde map en navigeer naar het.
   
   ```
   $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/
   
   $ cd /openfoam/sloshingTank3D
   ```
4. Wanneer u de standaardparameters van dit voorbeeld gebruikt, kan duren tien minuten worden uitgevoerd, zodat u wijzigen van sommige parameters wilt mogelijk zodat deze sneller worden uitgevoerd. Een eenvoudige keuze is om de tijd stap variabelen deltaT en writeInterval in het systeem/controlDict-bestand te wijzigen. Dit bestand wordt alle ingevoerde gegevens met betrekking tot het beheren van tijd- en lezen en schrijven van oplossingsgegevens. U kunt bijvoorbeeld de waarde van deltaT van 0,05 op 0,5 en de waarde van writeInterval van 0,05 op 0,5 wijzigen.
   
   ![Wijzig de variabelen in stap][step_variables]
5. Geef de gewenste waarden voor de variabelen in het bestand system/decomposeParDict. In dit voorbeeld maakt gebruik van twee Linux-knooppunten elke met 8 kernen, dus numberOfSubdomains ingesteld op 16 en n van hierarchicalCoeffs naar (1 1 16), wat betekent dat OpenFOAM parallel met 16 processen worden uitgevoerd. Zie voor meer informatie [OpenFOAM User Guide: 3,4 uitgevoerd toepassingen parallel](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).
   
   ![Processen afbreken][decompose]
6. Voer de volgende opdrachten uit de directory sloshingTank3D voorbereiden van de voorbeeldgegevens.
   
   ```
   $ . $WM_PROJECT_DIR/bin/tools/RunFunctions
   
   $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict
   
   $ runApplication blockMesh
   
   $ cp 0/alpha.water.org 0/alpha.water
   
   $ runApplication setFields  
   ```
7. U ziet het hoofdknooppunt van dat de bestanden met voorbeeldgegevens worden gekopieerd naar C:\OpenFoam\sloshingTank3D. (C:\OpenFoam is de gedeelde map op het hoofdknooppunt.)
   
   ![Gegevensbestanden worden opgeslagen in het hoofdknooppunt][data_files]

### <a name="host-file-for-mpirun"></a>Hostbestand voor mpirun
In deze stap maakt u een hostbestand (een lijst van rekenknooppunten) die de **mpirun** opdracht gebruikt.

1. Op een van de Linux-knooppunten, door een bestand met de naam hostfile onder /openfoam, zodat dit bestand kan worden bereikt op /openfoam/hostfile op alle knooppunten van Linux te maken.
2. Schrijf uw Linux-knooppuntnamen in dit bestand. In dit voorbeeld wordt bevat het bestand de volgende namen:
   
   ```       
   SUSE12RDMA-LN1
   SUSE12RDMA-LN2
   ```
   
   > [!TIP]
   > U kunt dit bestand ook op C:\OpenFoam\hostfile maken op het hoofdknooppunt. Als u deze optie kiest, opslaan als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd naar behoren op de Linux-knooppunten.
   > 
   > 
   
   **Wrapper voor Bash-scripts**
   
   Als u veel Linux-knooppunten hebt en u wilt dat uw uit te voeren op slechts enkele van deze taak, is het niet een goed idee om een hostbestand vaste te gebruiken omdat u niet weet welke knooppunten wordt toegewezen aan uw project. In dit geval een bash script wrapper voor schrijven **mpirun** bestand met de host om automatisch te maken. U kunt een voorbeeld bash script wrapper aangeroepen hpcimpirun.sh aan het einde van dit artikel vinden en opslaan als /openfoam/hpcimpirun.sh. Dit voorbeeldscript doet het volgende:
   
   1. Stelt de omgevingsvariabelen voor **mpirun**, en sommige opdrachtparameters toevoegen voor het uitvoeren van de taak MPI via het netwerk RDMA. In dit geval wordt de volgende variabelen ingesteld:
      
      * I_MPI_FABRICS shm:dapl =
      * I_MPI_DAPL_PROVIDER weergeeft van een-v2-ib0 =
      * I_MPI_DYNAMIC_CONNECTION = 0
   2. Maakt een hostbestand volgens de omgeving variabele $CCP_NODES_CORES die is ingesteld door het hoofdknooppunt HPC wanneer de taak wordt geactiveerd.
      
      De indeling van $CCP_NODES_CORES volgt dit patroon:
      
      ```
      <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
      ```
      
      waar
      
      * `<Number of nodes>`-het aantal knooppunten dat is toegewezen aan deze taak.  
      * `<Name of node_n_...>`-de naam van elk knooppunt dat is toegewezen aan deze taak.
      * `<Cores of node_n_...>`-het aantal kernen op het knooppunt dat is toegewezen aan deze taak.
      
      Bijvoorbeeld, als de taak twee knooppunten om uit te voeren moet, is $CCP_NODES_CORES vergelijkbaar met
      
      ```
      2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
      ```
   3. Aanroepen van de **mpirun** opdracht en voegt u twee parameters toe aan de opdrachtregel.
      
      * `--hostfile <hostfilepath>: <hostfilepath>`-het pad van het script maakt bestand met de host
      * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-een omgevingsvariabele ingesteld door het hoofdknooppunt HPC Pack, waarin het aantal totale kernen toegewezen aan deze taak worden opgeslagen. In dit geval geeft het aantal processen voor **mpirun**.

## <a name="submit-an-openfoam-job"></a>Verzenden van een taak OpenFOAM
U kunt nu een taak in HPC Cluster Manager verzenden. U moet de hpcimpirun.sh script in de opdrachtregels voor enkele van de taken van de taak doorgeven.

1. Verbinding maken met het hoofdknooppunt van het cluster en start HPC Cluster Manager.
2. **In de Resource Management**, zorg ervoor dat de Linux-rekenknooppunten in de **Online** status. Als dat niet het geval is, selecteert u deze en klikt u op **Online brengen**.
3. In **Jobbeheer**, klikt u op **nieuwe taak**.
4. Voer een naam voor de taak zoals *sloshingTank3D*.
   
   ![Taakdetails][job_details]
5. In **taak resources**, kies het type resource als 'Knooppunt' en het Minimum ingesteld op 2. Deze configuratie wordt de taak uitgevoerd op twee Linux-knooppunten, elk met acht kernen in dit voorbeeld heeft.
   
   ![Taak resources][job_resources]
6. Klik op **taken bewerken** in de navigatiebalk aan de linkerkant en klik vervolgens op **toevoegen** een taak toevoegen aan de taak. Vier taken toevoegen aan de taak met de volgende regels en instellingen.
   
   > [!NOTE]
   > Met `source /openfoam/settings.sh` stelt u de OpenFOAM en MPI-runtime-omgevingen, zodat elk van de volgende taken vóór de opdracht OpenFOAM aangeroepen.
   > 
   > 
   
   * **Taak 1**. Voer **decomposePar** voor het genereren van de gegevensbestanden voor het werken met **interDyMFoam** parallel.
     
     * Eén knooppunt toewijzen aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
     
     Zie de volgende afbeelding. U configureren de resterende taken op dezelfde manier.
     
     ![Taak 1-details][task_details1]
   * **Taak 2**. Voer **interDyMFoam** parallel berekenen van de steekproef.
     
     * Twee knooppunten toewijzen aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
   * **Taak 3**. Voer **reconstructPar** de sets met mappen van de tijd van elke directory processor_N_ samenvoegen in één verzameling.
     
     * Eén knooppunt toewijzen aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
   * **Taak 4**. Voer **foamToEnsight** parallel te converteren van het resultaat OpenFOAM bestanden naar EnSight formatteren en de bestanden EnSight plaatsen in een map met de naam Ensight in de map voor de aanvraag.
     
     * Twee knooppunten toewijzen aan de taak
     * **Vanaf de opdrachtregel** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`
     * **Werkmap** -/ openfoam/sloshingTank3D
7. Voeg de afhankelijkheden toe aan deze taken in oplopende taakvolgorde.
   
   ![Taakafhankelijkheden][task_dependencies]
8. Klik op **indienen** deze taak uit te voeren.
   
   Standaard verzonden HPC Pack door de taak als uw huidige aangemelde gebruikersaccount. Nadat u op **indienen**, ziet u mogelijk een dialoogvenster waarin u de gebruikersnaam en wachtwoord invoeren.
   
   ![Taak referenties][creds]
   
   Onder bepaalde omstandigheden onthoudt HPC Pack u de gebruikersgegevens invoeren voordat u dit dialoogvenster niet weergeven. Voer de volgende opdracht achter de opdrachtprompt en verzend de taak zodat HPC Pack opnieuw weergeven.
   
   ```
   hpccred delcreds
   ```
9. De taak duurt van tien minuten tot enkele uren volgens de parameters die u hebt ingesteld voor het voorbeeld. In de heatmap ziet u de taak uitgevoerd op de Linux-knooppunten. 
   
   ![Heatmap][heat_map]
   
   Op elk knooppunt worden acht processen gestart.
   
   ![Linux-processen][linux_processes]
10. Wanneer de taak is voltooid, moet u resultaten van de taak vinden in mappen onder C:\OpenFoam\sloshingTank3D en de logboekbestanden op C:\OpenFoam.

## <a name="view-results-in-ensight"></a>De resultaten weergeven in EnSight
Optioneel gebruik [EnSight](https://www.ceisoftware.com/) visualiseren en analyseren van de resultaten van de taak OpenFOAM. Zie voor meer informatie over visualisatie en animatie in EnSight, [video handleiding](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1. Nadat u op het hoofdknooppunt EnSight hebt geïnstalleerd, start u deze.
2. Open C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.
   
   U ziet een vervangen in de viewer.
   
   ![Vervangen in EnSight][tank]
3. Maak een **Isosurface** van **internalMesh**, en kies vervolgens de variabele **alpha_water**.
   
   ![Een isosurface maken][isosurface]
4. De kleur voor **Isosurface_part** in de vorige stap hebt gemaakt. Bijvoorbeeld: Stel deze in op water blauw.
   
   ![Isosurface kleur bewerken][isosurface_color]
5. Maak een **Iso-volume** van **wanden** door te selecteren **wanden** in de **delen** paneel en klik op de **Isosurfaces** knop op de werkbalk.
6. Selecteer in het dialoogvenster **Type** als **Isovolume** en stelt het minimum van **Isovolume bereik** op 0,5. Klik op om de isovolume **maken met de geselecteerde onderdelen**.
7. De kleur voor **Iso_volume_part** in de vorige stap hebt gemaakt. Bijvoorbeeld: Stel deze in op grondige water blauw.
8. De kleur voor **wanden**. Bijvoorbeeld, deze instellen op transparante wit.
9. Klik nu op **afspelen** om de resultaten van de simulatie te bekijken.
   
    ![Resultaat van de vervangen][tank_result]

## <a name="sample-files"></a>Voorbeeldbestanden
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Voorbeeld XML-configuratiebestand voor implementatie van het cluster door PowerShell-script
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Voorbeeld van een bestand silent.cfg MPI installeren
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

### <a name="sample-settingssh-script"></a>Voorbeeldscript settings.sh
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


### <a name="sample-hpcimpirunsh-script"></a>Voorbeeldscript hpcimpirun.sh
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
