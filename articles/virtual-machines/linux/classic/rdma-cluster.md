---
title: Een Linux RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen | Microsoft Docs
description: Een Linux-cluster van de grootte H16r, H16mr, A8 of A9-VM's naar het Azure RDMA-netwerk gebruiken voor het uitvoeren van MPI-apps maken
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/11/2018
ms.author: danlep
ms.openlocfilehash: 471fd4095fe45e76f94df8c61a07eeb9bbc1c120
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990724"
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Een Linux RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen

Meer informatie over het instellen van een Linux RDMA-cluster in Azure met [High performance computing-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) parallelle Message Passing Interface (MPI)-toepassingen uit te voeren. In dit artikel bevat stappen voor het voorbereiden van een installatiekopie van Linux HPC Intel MPI uitvoeren op een cluster. Nadat de voorbereiding, implementeert u een cluster met virtuele machines met behulp van deze installatiekopie en een van de [RDMA-compatibele Azure-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances). Het cluster gebruiken voor het uitvoeren van MPI-toepassingen die efficiënt via een lage latentie en hoge doorvoer netwerk op basis van technologie voor remote direct memory access (RDMA communiceren).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager](../../../azure-resource-manager/resource-manager-deployment-model.md) en klassiek. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

## <a name="cluster-deployment-options"></a>Cluster-implementatieopties
Hieronder vindt u verschillende methoden kunt u een Linux RDMA-cluster maken met of zonder een Taakplanner.

* **Azure CLI-scripts**: Zie verderop in dit artikel, gebruikt u de [Azure-opdrachtregelinterface](../../../cli-install-nodejs.md) (CLI) om een script van de implementatie van een cluster met RDMA-compatibele virtuele machines. De CLI in Service Management-modus maakt de clusterknooppunten opeenvolgend in het klassieke implementatiemodel, dus veel compute-knooppunten te implementeren kan enkele minuten duren. Implementeer de virtuele machines in dezelfde cloudservice zodat de RDMA-netwerkverbinding wanneer u het klassieke implementatiemodel.
* **Azure Resource Manager-sjablonen**: U kunt ook het implementatiemodel van Resource Manager gebruiken om een cluster van RDMA-compatibele virtuele machines die verbinding met de RDMA-netwerk maakt te implementeren. U kunt [uw eigen sjabloon maken](../../../resource-group-authoring-templates.md), of Controleer de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor sjablonen bijgedragen door Microsoft of de community van de oplossing die u wilt implementeren. Resource Manager-sjablonen kunnen bieden een snelle en betrouwbare manier om een Linux-cluster te implementeren. Als u wilt de RDMA-netwerkverbinding inschakelen wanneer u de Resource Manager-implementatiemodel, de virtuele machines in de dezelfde beschikbaarheidsset of virtuele-machineschaalset te implementeren.
* **HPC Pack**: een Microsoft HPC Pack-cluster maken in Azure en het toevoegen van RDMA-compatibele rekenknooppunten waarop een ondersteunde Linux-distributie voor toegang tot de RDMA-netwerk wordt uitgevoerd. Zie voor meer informatie, [aan de slag met Linux-rekenknooppunten in een HPC Pack-cluster in Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Voorbeeld van implementatie van de stappen in het klassieke model
De volgende stappen laten zien hoe de Azure CLI gebruiken voor een SUSE Linux Enterprise Server (SLES) 12 HPC VM implementeren vanaf de Azure Marketplace, aanpassen en een aangepaste VM-installatiekopie te maken. Vervolgens gebruikt u de installatiekopie van het script voor de implementatie van een cluster met RDMA-compatibele virtuele machines.

> [!TIP]
> Gebruik dezelfde stappen voor het implementeren van een cluster met RDMA-compatibele virtuele machines gebaseerd op CentOS gebaseerde HPC-installatiekopieën in de Azure Marketplace. Sommige stappen verschillen enigszins die u hebt genoteerd.
>

### <a name="prerequisites"></a>Vereisten
* **Clientcomputer**: U moet een Mac, Linux of Windows-clientcomputer om te communiceren met Azure. Deze stappen wordt ervan uitgegaan dat u een Linux-client.
* **Azure-abonnement**: als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten. Voor grotere clusters, kunt u een abonnement op gebruiksbasis of andere Aankoopopties.
* **Beschikbaarheid van VM-grootte**: Controleer [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor beschikbaarheid van de H-serie of andere RDMA-compatibele VM-grootten in Azure-regio's.
* **Quotum voor kerngeheugens**: mogelijk moet u vergroot het quotum voor kernen om een cluster van rekenintensieve VM's te implementeren. Bijvoorbeeld, moet u ten minste 128 kernen als u implementeren 8 A9-VM's wilt, zoals wordt weergegeven in dit artikel. Uw abonnement kan het aantal kernen dat u in een bepaalde groottefamilie VM implementeren kunt, met inbegrip van de H-serie ook beperken. Om aan te vragen een quotaverhoging [opent u een ondersteuningsaanvraag online klant](../../../azure-supportability/how-to-create-azure-support-request.md) gratis.
* **Azure CLI**: [installeren](../../../cli-install-nodejs.md) de Azure CLI en [verbinding maken met uw Azure-abonnement](/cli/azure/authenticate-azure-cli) vanaf de clientcomputer.

Bekijk ook overwegingen voor de implementatie de [RDMA-compatibele Azure-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#rdma-capable-instances).

### <a name="provision-an-sles-12-hpc-vm"></a>Een SLES 12 HPC VM inrichten
Na het aanmelden bij Azure met de Azure CLI, voert `azure config list` om te bevestigen dat de uitvoer ziet u Service Management-modus. Als dit niet het geval is, moet u de modus instellen door het uitvoeren van deze opdracht:

    azure config mode asm


Typ het volgende als u wilt weergeven van alle abonnementen die u gemachtigd bent om te gebruiken:

    azure account list

Het huidige actieve abonnement wordt geïdentificeerd met `Current` ingesteld op `true`. Als dit abonnement is niet de versie die u wilt gebruiken om het cluster te maken, stelt u de juiste abonnements-ID als het actieve abonnement:

    azure account set <subscription-Id>

Als u wilt zien van de openbaar beschikbare installatiekopieën voor SLES 12 HPC in Azure, voert u een opdracht als volgt, ervan uitgaande dat uw shell-omgeving ondersteunt **grep**:

    azure vm image list | grep "suse.*hpc"

Richt een RDMA-compatibele virtuele machine met een SLES 12 SP3 HPC-installatiekopie door het uitvoeren van een opdracht als volgt uit:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-v20170913

Waar:

* De grootte (A9 in dit voorbeeld) is een van de RDMA-compatibele VM-grootten.
* De externe SSH-poortnummer (22 in dit voorbeeld de standaardinstelling voor SSH is) is een geldig poortnummer. De interne SSH-poortnummer is ingesteld op 22.
* Een nieuwe cloudservice wordt gemaakt in de Azure-regio opgegeven door de locatie. Geef een locatie op waarin de VM-grootte die u kiest beschikbaar, zoals 'VS-West is.
* Voor ondersteuning met prioriteit SUSE (die leidt tot extra kosten), de naam van de SLES 12-installatiekopie op dit moment mag een van de opties met `priority` in de naam, zoals: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp3-hpc-priority-v20170913`

### <a name="customize-the-vm"></a>De virtuele machine aanpassen
Nadat de virtuele machine is voltooid wanneer het inrichten, SSH naar de virtuele machine met behulp van de virtuele machine externe IP-adres (of DNS-naam) en de externe poort nummer dat u hebt geconfigureerd en vervolgens aan te passen. Zie voor de verbindingsgegevens, [Meld u aan een virtuele machine waarop Linux wordt uitgevoerd bij het](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Opdrachten uitvoeren als de gebruiker die u hebt geconfigureerd op de virtuele machine, tenzij de toegang tot de hoofdmap is vereist om een stap te voltooien.

> [!IMPORTANT]
> Microsoft Azure biedt geen toegang tot de hoofdmap virtuele Linux-machines. Met beheerdersrechten om toegang te krijgen als verbonden als een gebruiker met de virtuele machine, kunt u opdrachten uitvoeren met behulp van `sudo`.
>
>

* **Updates**: updates installeren met zypper. U kunt ook voor het installeren van hulpprogramma's voor NFS.

  > [!IMPORTANT]
  > In een SLES 12 HPC VM, is het raadzaam dat u kernelupdates, die leiden problemen met de Linux RDMA stuurprogramma's tot kunnen niet van toepassing.
  >
  >
* **Intel MPI**: voltooit de installatie van Intel MPI op de virtuele machine met de volgende opdracht:

    ```bash
    sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
    ```

* **Geheugen vergrendelen**: voor MPI-codes voor het vergrendelen van het beschikbare geheugen voor RDMA, toevoegen of wijzigen van de volgende instellingen in het bestand /etc/security/limits.conf. U moet toegang tot de hoofdmap om dit bestand te bewerken.

    ```bash
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Voor testdoeleinden kunt u ook memlock instellen op onbeperkt. Bijvoorbeeld: `* hard memlock unlimited`. Zie voor meer informatie, [meest bekende methoden voor instelling vergrendeld geheugengrootte](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **SSH-sleutels voor SLES virtuele machines**: genereren van SSH-sleutels vertrouwensrelatie voor uw gebruikersaccount tussen de rekenknooppunten in de SLES cluster bij het uitvoeren van MPI-opdrachten. Als u een VM op basis van CentOS HPC hebt geïmplementeerd, niet volgt u deze stap. Zie de instructies verderop in dit artikel voor het instellen van de configuratie SSH-vertrouwensrelatie tussen de clusterknooppunten nadat u de installatiekopie maken en implementeren van het cluster.

  Voer voor het maken van SSH-sleutels de `ssh-keygen` opdracht. Wanneer u wordt gevraagd voor invoer, selecteert u **Enter** voor het genereren van de sleutels in de standaardlocatie zonder het instellen van een wachtwoord.

  De openbare sleutel toevoegen aan het bestand authorized_keys voor bekende openbare sleutels.

  ```bash
  cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  ```

  In de map ~/.ssh bewerken of maken van de `config` bestand. Geef het IP-adresbereik van het particuliere netwerk die u wilt gebruiken in Azure (10.32.0.0/16 in dit voorbeeld):

  ```bash
  host 10.32.0.*
  StrictHostKeyChecking no
  ```

  Vermeld het particuliere netwerk-IP-adres van elke virtuele machine in uw cluster ook als volgt:

  ```bash
  host 10.32.0.1
  StrictHostKeyChecking no
  host 10.32.0.2
  StrictHostKeyChecking no
  host 10.32.0.3
  StrictHostKeyChecking no
  ...
  ```

  > [!NOTE]
  > Configureren van `StrictHostKeyChecking no` een mogelijk beveiligingsrisico kunt maken als een specifiek IP-adres of adresbereik niet is opgegeven.
  >
  >
* **Toepassingen**: installeert u alle toepassingen die u nodig hebt of andere aanpassingen uitvoeren voordat u de installatiekopie vastlegt.

### <a name="capture-the-image"></a>De installatiekopie vastleggen
Als u wilt de installatiekopie vastlegt, moet u eerst de volgende opdracht uitvoeren op de Linux-VM. Met deze opdracht deprovisions van de virtuele machine, maar behoudt gebruikersaccounts en SSH-sleutels die u hebt ingesteld.

```bash
sudo waagent -deprovision
```

Voer de volgende Azure CLI-opdrachten om vast te leggen van de installatiekopie van het vanaf uw clientcomputer. Zie voor meer informatie, [een klassieke virtuele Linux-machine als afbeelding vastleggen](capture-image-classic.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nadat u deze opdrachten uitvoeren, de VM-installatiekopie wordt vastgelegd voor het gebruik en de virtuele machine is verwijderd. U hebt nu uw aangepaste installatiekopie die gereed is om een cluster te implementeren.

### <a name="deploy-a-cluster-with-the-image"></a>Een cluster met de installatiekopie implementeren
Wijzig het volgende Bash-script met de juiste waarden voor uw omgeving en deze uitvoeren vanaf uw clientcomputer. Omdat Azure de virtuele machines worden in het klassieke implementatiemodel implementeert, duurt het enkele minuten de acht A9-virtuele machines voorgesteld in dit script te implementeren.

```bash
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a classic VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>;
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Overwegingen voor een CentOS HPC-cluster
Als u een op basis van een van de HPC op basis van CentOS-installatiekopieën in de Azure Marketplace in plaats van SLES 12 voor HPC-cluster instellen wilt, volgt u de algemene stappen in de voorgaande sectie. Houd rekening met de volgende verschillen bij het inrichten en de virtuele machine configureren:

- Intel MPI is al geïnstalleerd op een virtuele machine ingericht vanuit een HPC op basis van CentOS-installatiekopie.
- Instellingen voor het geheugen vergrendelen zijn al toegevoegd aan de /etc/security/limits.conf-bestand van de virtuele machine.
- Geen SSH-sleutels op de virtuele machine die u inricht voor het vastleggen van worden gegenereerd. In plaats daarvan wordt aangeraden het instellen van verificatie op basis van gebruikers nadat u het cluster hebt geïmplementeerd. Zie de volgende sectie voor meer informatie.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Instellen van de configuratie van SSH-vertrouwensrelatie op het cluster
Op een CentOS gebaseerde HPC-cluster, er zijn twee methoden voor het instellen van een vertrouwensrelatie tussen de rekenknooppunten: verificatie op basis van de host en verificatie op basis van gebruiker. Verificatie op basis van een host is buiten het bereik van dit artikel en over het algemeen moet worden uitgevoerd via een script-extensies tijdens de implementatie. Verificatie op basis van een gebruiker is handig voor het instellen van een vertrouwensrelatie na de implementatie en vereist het genereren en het delen van SSH-sleutels tussen de compute-knooppunten in het cluster. Deze methode staat bekend als de configuratie SSH-aanmelding en is vereist bij het uitvoeren van MPI-opdrachten.

Een voorbeeld van een script `user_authentication.sh` om in te schakelen van verificatie op basis van een gebruiker op een CentOS gebaseerde HPC cluster volgt:

```bash
#!/bin/bash
# For CentOS user must first install epel-release, sshpass, and nmap (sshpass and nmap are available from epel-release for CentOS)

# usage ./user_authentication.sh [username] [password] [internalIP prefix]
# ./user_authentication.sh azureuser Azure@123 10.32.0
USER=$1
PASS=$2
IPPRE=$3
HEADNODE=`hostname`

mkdir -p .ssh
echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''

echo 'Host *' >> .ssh/config
echo 'StrictHostKeyChecking no' >> .ssh/config
chmod 400 .ssh/config
chown azureuser:azureuser /home/azureuser/.ssh/config

nmap -sn $IPPRE.* | grep $IPPRE. | awk '{print $5}' > nodeips.txt
for NAME in `cat nodeips.txt`; do sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'hostname' >> nodenames.txt;done

NAMES=`cat nodenames.txt` #names from names.txt file
for NAME in $NAMES; do
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 /home/$USER/nodenames.txt $USER@$NAME:/home/$USER/
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "mkdir .ssh && chmod 700 .ssh"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME "echo -e  'y\n' | ssh-keygen -f .ssh/id_rsa -t rsa -N ''"
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'touch /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo "Host *" >  /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'echo StrictHostKeyChecking no >> /home/'$USER'/.ssh/config'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 400 /home/'$USER'/.ssh/config'
    cat .ssh/id_rsa.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'cat >> .ssh/authorized_keys'
    sshpass -p $PASS scp -o "StrictHostKeyChecking no" -o ConnectTimeout=2 $USER@$NAME:/home/$USER/.ssh/id_rsa.pub .ssh/sub_node.pub

    for SUBNODE in `cat nodeips.txt`; do
         sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'mkdir -p .ssh'
         cat .ssh/sub_node.pub | sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$SUBNODE 'cat >> .ssh/authorized_keys'
    done
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 700 .ssh/'
    sshpass -p $PASS ssh -o ConnectTimeout=2 $USER@$NAME 'chmod 640 .ssh/authorized_keys'
done
```

Voer dit script uit met behulp van de volgende stappen uit. U kunt ook wijzigen met dit script of een andere methode gebruiken om de configuratie van SSH-verificatie tussen de knooppunten van het compute stand te brengen.

Het script wordt uitgevoerd, moet u het voorvoegsel voor uw subnet IP-adressen weten. Het voorvoegsel ophalen door het uitvoeren van de volgende opdracht uit op een van de clusterknooppunten. De uitvoer ziet er ongeveer als 10.1.3.5 en het voorvoegsel is de/m 10.1.3 gedeelte.

```bash
ifconfig eth0 | grep -w inet | awk '{print $2}'
```

Voer nu het script uit met behulp van drie parameters: de algemene gebruikersnaam op de rekenknooppunten, de algemene wachtwoord voor de gebruiker op de rekenknooppunten en het subnetvoorvoegsel die is geretourneerd met de vorige opdracht.

```bash
./user_authentication.sh <myusername> <mypassword> 10.1.3
```

Dit script doet het volgende:

* Hiermee maakt u een map op de hostknooppunt met de naam .ssh, die is vereist voor aanmelding zonder wachtwoord uitproberen.
* Hiermee maakt u een configuratiebestand in de map .SSH geeft u de configuratie Meld u aan bij aanmelding vanaf elk knooppunt in het cluster toestaan.
* Bestanden met de knooppuntnamen van en het knooppunt IP-adressen voor alle knooppunten in het cluster maakt. Deze bestanden zijn gebleven nadat het script wordt uitgevoerd voor later gebruik.
* Hiermee maakt u een persoonlijke en openbare sleutelpaar voor elk clusterknooppunt (met inbegrip van het hostknooppunt) en vermeldingen in het bestand authorized_keys maakt.

> [!WARNING]
> Uitvoeren van dit script kunt maken van een mogelijk beveiligingsrisico. Zorg ervoor dat de gegevens van de openbare sleutel in ~/.ssh niet wordt gedistribueerd.
>

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI worden uitgevoerd op een eenvoudige cluster met twee knooppunten
Als u dit nog niet hebt gedaan, eerst stelt u de omgeving voor Intel MPI.

```bash
# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Een MPI-opdracht uitvoeren
Een MPI-opdracht uitvoeren op een van de rekenknooppunten om weer te geven dat MPI correct is geïnstalleerd en communiceren kan tussen ten minste dat twee rekenknooppunten. De volgende **mpirun** opdracht wordt uitgevoerd de **hostnaam** opdracht op twee knooppunten. Voor de `-hosts` parameter, de IP-adressen van twee knooppunten in het Azure VNet (bijvoorbeeld 10.32.0.4,10.32.0.5) doorgeven.

```bash
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Uw uitvoer moet de namen van alle knooppunten die u hebt doorgegeven als invoer voor de lijst `-hosts`. Bijvoorbeeld, een **mpirun** opdracht met twee knooppunten retourneert uitvoer als volgt:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Voer een MPI-benchmark
De volgende Intel MPI-opdracht wordt uitgevoerd een benchmark pingpong om te controleren of de configuratie van het cluster en de verbinding met de RDMA-netwerk.

```bash
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Op een cluster werken met twee knooppunten ziet u de volgende uitvoer. Op het netwerk van Azure RDMA, verwachte latentie op of onder 3 microseconden voor het bericht het formaat van maximaal 512 bytes.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 6 17:16:46 2018
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```
### <a name="sample-mpi-run-script"></a>Voorbeeld van MPI-script uitvoeren
Hier volgt een Bash-voorbeeldscript voor het configureren van de variabelen die nodig zijn voor het uitvoeren van een MPI-toepassing. Wijzig het pad naar `mpivars.sh` zo nodig voor de installatie van Intel MPI.

```bash
#!/bin/bash -x

# For a SLES 12 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the MPI job. Substitute with values appropriate for your application.

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

De indeling van het bestand met de host is als volgt. Een regel voor elk knooppunt in het cluster toevoegen. Geef het particuliere IP-adressen van het virtuele netwerk eerder hebt gedefinieerd, niet de DNS-namen. Bijvoorbeeld, bevat het bestand voor twee hosts met IP-adressen 10.32.0.4 en 10.32.0.4 het volgende:

```bash
10.32.0.4:16
10.32.0.5:16
```


## <a name="next-steps"></a>Volgende stappen
* Implementeren en uw Linux MPI-toepassingen uitvoeren op uw Linux-cluster.
* Zie de [bibliotheekdocumentatie van Intel MPI](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) voor hulp bij Intel MPI.
* Probeer een [quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) voor het maken van een Intel Lustre-cluster met behulp van een HPC op basis van CentOS-installatiekopie. 
