---
title: NAMD met Microsoft HPC Pack op de virtuele Linux-machines | Microsoft Docs
description: Een Microsoft HPC Pack cluster in Azure implementeren en uitvoeren van een simulatie NAMD met charmrun op meerdere rekenknooppunten voor Linux
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 76072c6b-ac35-4729-ba67-0d16f9443bd7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 10/13/2016
ms.author: danlep
ms.openlocfilehash: 0c0b9875b4153edcc0ec0096577d041d394a842f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>NAMD uitvoeren met Microsoft HPC Pack op Linux-rekenknooppunten in Azure
Dit artikel ziet u een manier voor het uitvoeren van een werkbelasting Linux high performance computing (HPC) op virtuele machines in Azure. Hier kunt u instellen van een [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster in Azure met Linux-rekenknooppunten en voer een [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulatie te berekenen en de structuur van een systeem grote biomoleculaire visualiseren.  

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (voor Nanoscale moleculaire Dynamics programma) is een parallelle moleculaire dynamics-pakket dat is ontworpen voor de simulatie van grote biomoleculaire systemen miljoenen atomen met hoge prestaties. Voorbeelden van deze systemen zijn virussen, cel structuren en grote eiwitten. NAMD schaalt honderden kernen voor het typische simulaties en meer dan 500.000 kernen voor de grootste simulaties.
* **Microsoft HPC Pack** biedt functies voor het uitvoeren van grootschalige HPC en parallelle toepassingen in clusters van lokale computers of virtuele machines in Azure. Oorspronkelijk is ontwikkeld als een oplossing voor Windows HPC-workloads, HPC Pack nu ondersteunt Linux HPC-toepassingen uitvoert op Linux compute knooppunt virtuele machines in een cluster HPC Pack geïmplementeerd. Zie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md) voor een inleiding.

## <a name="prerequisites"></a>Vereisten
* **HPC Pack cluster met Linux-rekenknooppunten** -implementeren van een cluster met HPC Pack met Linux-rekenknooppunten in Azure met behulp van een [Azure Resource Manager-sjabloon](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) of een [Azure PowerShell-script](hpcpack-cluster-powershell-script.md). Zie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md) voor de vereisten en stappen voor beide opties. Als u de PowerShell-script-implementatie-optie kiest, raadpleegt u het voorbeeldconfiguratiebestand in de voorbeeldbestanden aan het einde van dit artikel. Dit bestand configureert een HPC Pack op basis van een Azure-cluster die bestaan uit een Windows Server 2012 R2-hoofdknooppunt en vier grootte grote CentOS 6.6-rekenknooppunten. Dit bestand aanpassen nodig zijn voor uw omgeving.
* **De software en zelfstudie bestanden NAMD** -NAMD downloaden van software voor Linux van de [NAMD](http://www.ks.uiuc.edu/Research/namd/) site (registratie vereist). In dit artikel is gebaseerd op NAMD versie 2.10 en maakt gebruik van de [Linux-x86_64 (64-bits Intel/AMD met Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) archief. Ook downloaden de [NAMD zelfstudie bestanden](http://www.ks.uiuc.edu/Training/Tutorials/#namd). De downloads tar-bestanden zijn en moet u een Windows-hulpprogramma voor het uitpakken van de bestanden op het hoofdknooppunt van het cluster. De bestanden wilt uitpakken, volg de instructies verderop in dit artikel. 
* **VMD** (optioneel): de resultaten van de taak NAMD, downloaden en installeren van het programma moleculaire visualisatie [VMD](http://www.ks.uiuc.edu/Research/vmd/) op een computer van uw keuze. De huidige versie is 1.9.2. Zie de VMD downloaden van de site aan de slag.  

## <a name="set-up-mutual-trust-between-compute-nodes"></a>Wederzijdse vertrouwensrelatie tussen de rekenknooppunten instellen
Een taak cross-knooppunt wordt uitgevoerd op meerdere Linux-knooppunten vereist dat de knooppunten die elkaar vertrouwen (door **rsh** of **ssh**). Wanneer u het cluster HPC Pack met het script voor Microsoft HPC Pack IaaS-implementatie maakt, het script permanente wederzijds vertrouwen voor de administrator-account dat u opgeeft automatisch ingesteld. Voor gebruikers die geen beheerder u in het domein van het cluster maakt, hebt u tijdelijke wederzijds vertrouwen tussen de knooppunten ingesteld als een taak aan hen is toegewezen. Vernietig vervolgens de relatie nadat de taak voltooid is. Om dit te doen voor elke gebruiker, bieden een RSA-sleutelpaar naar het cluster dat gebruikmaakt van HPC Pack om de vertrouwensrelatie te maken. Instructies volgen.

### <a name="generate-an-rsa-key-pair"></a>Een RSA-sleutelpaar niet genereren
Het is gemakkelijk voor het genereren van een RSA-sleutelpaar, die een openbare sleutel en een persoonlijke sleutel bevat door het uitvoeren van de Linux **ssh-keygen** opdracht.

1. Meld u op een Linux-computer.
2. Voer de volgende opdracht uit:
   
   ```bash
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
1. [Verbinding maken met extern bureaublad](../../windows/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) met het hoofdknooppunt VM gebruikmaken van het domein referenties u opgegeven bij de implementatie van het cluster (bijvoorbeeld hpc\clusteradmin). U beheren het cluster vanaf het hoofdknooppunt.
2. Gebruik standaard Windows Server-procedures om een domeingebruikersaccount maken in Active Directory-domein van het cluster. Bijvoorbeeld, gebruik het hulpprogramma Active Directory-gebruikers en Computers op het hoofdknooppunt. De voorbeelden in dit artikel wordt ervan uitgegaan dat u een domeingebruiker die met de naam hpcuser in het domein hpclab (hpclab\hpcuser) maken.
3. Voeg de domeingebruiker aan het cluster HPC Pack als de gebruiker van een cluster. Zie voor instructies [toevoegen of verwijderen cluster gebruikers](https://technet.microsoft.com/library/ff919330.aspx).
4. Maak een bestand met de naam C:\cred.xml en kopieer de RSA-sleutelgegevens in de App. In de voorbeeldbestanden aan het einde van dit artikel vindt u een voorbeeld.
   
   ```
   <ExtendedData>
     <PrivateKey>Copy the contents of private key here</PrivateKey>
     <PublicKey>Copy the contents of public key here</PublicKey>
   </ExtendedData>
   ```
5. Open een opdrachtprompt en voer de volgende opdracht om de gegevens van de referenties voor het account hpclab\hpcuser instellen. U gebruikt de **extendeddata** -parameter voor de naam van het bestand C:\cred.xml u hebt gemaakt voor de belangrijkste gegevens doorgeeft.
   
   ```command
   hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
   ```
   
   Deze opdracht is voltooid zonder uitvoer. Na het instellen van de referenties voor de gebruikersaccounts die u wilt uitvoeren van taken, sla het bestand cred.xml in een veilige locatie of verwijder deze.
6. Als u de RSA-sleutelpaar gegenereerd op een van uw Linux-knooppunten, moet u de sleutels verwijderen nadat u klaar bent met het gebruik ervan. HPC Pack stelt geen wederzijdse vertrouwensrelatie Als het een bestaand id_rsa bestand of id_rsa.pub bestand gevonden.

> [!IMPORTANT]
> Wordt niet aanbevolen als de Clusterbeheerder van een een Linux-taak wordt uitgevoerd op een gedeelde cluster, omdat een taak die is verzonden door een beheerder wordt uitgevoerd onder het hoofdaccount op de Linux-knooppunten. Een taak die is verzonden door een gebruiker die geen beheerder wordt uitgevoerd onder een lokale Linux-gebruikersaccount met dezelfde naam als de gebruiker van de taak. In dit geval stelt HPC Pack wederzijds vertrouwen voor deze gebruiker Linux op alle knooppunten die zijn toegewezen aan de taak. U kunt de Linux-gebruiker handmatig op de Linux-knooppunten instellen voordat de taak wordt uitgevoerd of HPC Pack de gebruiker wordt automatisch gemaakt wanneer de taak wordt verzonden. Als HPC Pack de gebruiker maakt, verwijderd HPC Pack nadat de taak is voltooid. Als u beveiligingsrisico, worden de sleutels worden verwijderd nadat de taak is voltooid op de knooppunten.
> 
> 

## <a name="set-up-a-file-share-for-linux-nodes"></a>Instellen van een bestandsshare voor Linux-knooppunten
Nu instellen van een SMB-bestandsshare en de gedeelde map op alle Linux-knooppunten kunnen de Linux-knooppunten toegang tot bestanden met een algemeen pad NAMD koppelen. Hieronder volgen de stappen voor het koppelen van een gedeelde map op het hoofdknooppunt. Een share wordt aanbevolen voor distributies zoals CentOS 6.6 die de Azure File service momenteel niet ondersteund. Als uw Linux-knooppunten een Azure-bestandsshare ondersteunt, Zie [Azure File storage gebruiken met Linux](../../../storage/files/storage-how-to-use-files-linux.md). Zie voor extra opties met HPC Pack delen van bestanden, [aan de slag met Linux-rekenknooppunten in een HPC Pack-Cluster in Azure](hpcpack-cluster.md).

1. Maak een map op het hoofdknooppunt en voor iedereen delen door het instellen van machtigingen voor lezen/schrijven. In dit voorbeeld \\ \\CentOS66HN\Namd is de naam van de map, waarbij CentOS66HN de hostnaam van het hoofdknooppunt.
2. Maak een submap met de naam namd2 in de gedeelde map. In namd2, maakt u een nieuwe submap met de naam namdsample.
3. Pak de NAMD-bestanden in de map met behulp van een Windows-versie van **tar** of een ander Windows-hulpprogramma dat met TAR-archieven werkt. 
   
   * Pak het tar-archief NAMD naar \\ \\CentOS66HN\Namd\namd2.
   * Pak de bestanden van de zelfstudie onder \\ \\CentOS66HN\Namd\namd2\namdsample.
4. Open een Windows PowerShell-venster en voer de volgende opdrachten om te koppelen van de gedeelde map op de Linux-knooppunten.
   
    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2
   
    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

De eerste opdracht maakt een map met de naam /namd2 op alle knooppunten in de groep LinuxNodes. De tweede opdracht koppelt de gedeelde map //CentOS66HN/Namd/namd2 naar de map met dir_mode en file_mode bits ingesteld op 777. De *gebruikersnaam* en *wachtwoord* in de opdracht moet de referenties van een gebruiker op het hoofdknooppunt.

> [!NOTE]
> De '\`' symbool in de tweede opdracht is een escapeteken voor PowerShell. "\`, ' betekent dat de","(kommateken) maakt deel uit van de opdracht.
> 
> 

## <a name="create-a-bash-script-to-run-a-namd-job"></a>Een Bash-script om uit te voeren van een taak NAMD maken
De taak NAMD moet een *nodelist* bestand voor **charmrun** om te bepalen het aantal knooppunten moet worden gebruikt bij het starten van NAMD processen. U gebruikt een Bash-script die wordt uitgevoerd en genereert de knooppuntenbestand **charmrun** met deze knooppuntenbestand. Vervolgens dient u een taak NAMD in HPC Cluster Manager waarmee dit script wordt aangeroepen.

Met een teksteditor van uw keuze, een Bash-script maken in de /namd2-map met de programmabestanden NAMD en noem deze hpccharmrun.sh. Voor een snel testen van het concept, kopieert u het voorbeeldscript hpccharmrun.sh verstrekt aan het einde van dit artikel en Ga naar [verzenden van een taak NAMD](#submit-a-namd-job).

> [!TIP]
> Sla het script als een tekstbestand met Linux regeleinden (alleen LF, niet CR LF). Dit zorgt ervoor dat deze wordt uitgevoerd naar behoren op de Linux-knooppunten.
> 
> 

Hieronder vindt u meer informatie over de werking van dit bash-script. 

1. Sommige variabelen definiëren.
   
   ```bash
   #!/bin/bash
   
   # The path of this script
   SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
   # Charmrun command
   CHARMRUN=${SCRIPT_PATH}/charmrun
   # Argument of ++nodelist
   NODELIST_OPT="++nodelist"
   # Argument of ++p
   NUMPROCESS="+p"
   ```
2. Knooppunt-informatie ophalen uit de omgevingsvariabelen. $NODESCORES slaat een lijst met woorden van $CCP_NODES_CORES splitsen. $COUNT is de grootte van $NODESCORES.
   
   ```
   # Get node information from the environment variables
   NODESCORES=(${CCP_NODES_CORES})
   COUNT=${#NODESCORES[@]}
   ```    
   
   De indeling voor de $CCP_NODES_CORES variabele is als volgt:
   
   ```
   <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
   ```
   
   Deze variabele bevat het totale aantal knooppunten, knooppuntnamen en het aantal kernen op elk knooppunt dat wordt toegewezen aan de taak. Bijvoorbeeld, als de taak 10 kernen dat moet moet worden uitgevoerd, lijkt de waarde van $CCP_NODES_CORES op:
   
   ```
   3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
   ```
3. Als de $CCP_NODES_CORES variabele niet is ingesteld, start u **charmrun** rechtstreeks. (Dit moet alleen worden uitgevoerd als u dit script rechtstreeks op uw Linux-knooppunten uitvoert.)
   
   ```
   if [ ${COUNT} -eq 0 ]
   then
     # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
     #echo ${CHARMRUN} $*
     ${CHARMRUN} $*
   ```
4. Of maak een knooppuntenbestand voor **charmrun**.
   
   ```
   else
     # Create the nodelist file
     NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$
   
     # Write the head line
     echo "group main" > ${NODELIST_PATH}
   
     # Get every node name and number of cores and write into the nodelist file
     I=1
     while [ ${I} -lt ${COUNT} ]
     do
         echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
         let "I=${I}+2"
     done
   ```
5. Voer **charmrun** met de knooppuntenbestand ophalen van de status van het resultaat, en verwijder de knooppuntenbestand aan het einde.
   
   ${CCP_NUMCPUS} is een andere omgevingsvariabele ingesteld door het hoofdknooppunt HPC Pack. Het aantal totale kernen toegewezen aan deze taak worden opgeslagen. We gebruiken deze om het aantal processen voor charmrun te geven.
   
   ```
   # Run charmrun with nodelist arg
   #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
   
   RTNSTS=$?
   rm -f ${NODELIST_PATH}
   fi
   
   ```
6. Afgesloten met de **charmrun** status retourneren.
   
   ```
   exit ${RTNSTS}
   ```

Hieronder vindt u de informatie in de knooppuntenbestand dat het script wordt gegenereerd:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Bijvoorbeeld:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Verzenden van een taak NAMD
U bent nu klaar om te verzenden van een taak NAMD in HPC Cluster Manager.

1. Verbinding maken met het hoofdknooppunt van het cluster en start HPC Cluster Manager.
2. In **bronbeheer**, zorg ervoor dat de Linux-rekenknooppunten in de **Online** status. Als dat niet het geval is, selecteert u deze en klikt u op **Online brengen**.
3. In **Jobbeheer**, klikt u op **nieuwe taak**.
4. Voer een naam voor de taak zoals *hpccharmrun*.
   
   ![Nieuwe HPC-taak][namd_job]
5. Op de **taakdetails** pagina onder **taak Resources**, selecteer het type resource als **knooppunt** en stel de **Minimum** 3. , wordt de taak uitgevoerd op drie Linux-knooppunten en elk knooppunt vier kernen heeft.
   
   ![Taak resources][job_resources]
6. Klik op **taken bewerken** in de navigatiebalk aan de linkerkant en klik vervolgens op **toevoegen** een taak toevoegen aan de taak.    
7. Op de **taakdetails en i/o-omleiding** pagina, stelt u de volgende waarden:
   
   * **Opdrachtregel**-
     `/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`
     
     > [!TIP]
     > De bovenstaande opdrachtregel is één opdracht zonder regeleinden. Deze loopt om weergegeven op meerdere regels onder **opdrachtregel**.
     > 
     > 
   * **Werkmap** -/namd2
   * **Minimale** - 3
     
     ![Taakdetails][task_details]
     
     > [!NOTE]
     > De werkmap hier ingestelde omdat **charmrun** probeert om te navigeren naar de dezelfde werkmap op elk knooppunt. Als de werkmap niet is ingesteld, wordt de opdracht in HPC Pack gestart in een willekeurige naam map gemaakt op een van de Linux-knooppunten. Dit zorgt ervoor dat de volgende fout op de andere knooppunten: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` om te voorkomen dat dit probleem, een mappad die kan worden geopend door alle knooppunten de werkmap opgeven.
     > 
     > 
8. Klik op **OK** en klik vervolgens op **indienen** deze taak uit te voeren.
   
   Standaard verzonden HPC Pack door de taak als uw huidige aangemelde gebruikersaccount. Een dialoogvenster vragen u de gebruikersnaam en wachtwoord invoeren nadat u op **indienen**.
   
   ![Taak referenties][creds]
   
   Onder bepaalde omstandigheden onthoudt HPC Pack u de gebruikersgegevens invoeren voordat u dit dialoogvenster niet weergeven. Voer de volgende opdracht achter de opdrachtprompt en verzend de taak zodat HPC Pack opnieuw weergeven.
   
   ```command
   hpccred delcreds
   ```
9. De taak duurt enkele minuten duren.
10. Zoeken naar het logbestand van de taak op \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log en de uitvoerbestanden in \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.
11. Start desgewenst VMD om de taak van de resultaten weer te geven. De stappen voor het visualiseren van de NAMD uitvoer-bestanden (in dit geval een ubiquitin eiwit molecuul in een bol water) zich buiten het bereik van dit artikel. Zie [NAMD zelfstudie](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) voor meer informatie.
    
    ![Resultaten van de taak][vmd_view]

## <a name="sample-files"></a>Voorbeeldbestanden
### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Voorbeeld XML-configuratiebestand voor implementatie van het cluster door PowerShell-script
```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Voorbeeldscript hpccharmrun.sh
```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 

<!--Image references-->
[keygen]:media/hpcpack-cluster-namd/keygen.png
[keys]:media/hpcpack-cluster-namd/keys.png
[namd_job]:media/hpcpack-cluster-namd/namd_job.png
[job_resources]:media/hpcpack-cluster-namd/job_resources.png
[creds]:media/hpcpack-cluster-namd/creds.png
[task_details]:media/hpcpack-cluster-namd/task_details.png
[vmd_view]:media/hpcpack-cluster-namd/vmd_view.png
