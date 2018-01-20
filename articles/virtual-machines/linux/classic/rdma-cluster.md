---
title: Instellen van een cluster Linux RDMA MPI-toepassingen uitvoeren | Microsoft Docs
description: Maak een Linux-cluster met een grootte van H16r, H16mr, A8 of A9-VM's met het netwerk van Azure RDMA MPI-apps uitvoeren
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 01834bad-c8e6-48a3-b066-7f1719047dd2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: danlep
ms.openlocfilehash: 428dc1e8ba03ba17e348a33a33b5cf5e6118a43c
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Een Linux RDMA-cluster instellen voor het uitvoeren van MPI-toepassingen
Meer informatie over het instellen van een Linux RDMA-cluster in Azure met [hoge prestaties compute-VM-grootten](../sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) parallelle Message Passing Interface (MPI)-toepassingen uit te voeren. Dit artikel bevat stappen voor het voorbereiden van een installatiekopie Linux HPC Intel MPI uitvoeren op een cluster. Nadat de voorbereiding, moet u een cluster van virtuele machines met behulp van deze installatiekopie en een van de RDMA-compatibele Azure VM-grootten (H16mr momenteel H16r A8 of A9) implementeert. De cluster gebruiken voor het uitvoeren van MPI-toepassingen die efficiënt via een lage latentie, een hoge gegevensdoorvoer netwerk op basis van remote direct memory access (RDMA)-technologie communiceren.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) en klassieke. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

## <a name="cluster-deployment-options"></a>Cluster-implementatieopties
Hieronder vindt u een Linux RDMA-cluster maken met of zonder een Taakplanner kunt u methoden.

* **Azure CLI-scripts**: Zie verderop in dit artikel, gebruiken de [Azure-opdrachtregelinterface](../../../cli-install-nodejs.md) (CLI) script voor de implementatie van een cluster met RDMA-compatibele virtuele machines. De CLI in Service Management-modus maakt de clusterknooppunten opeenvolgend in het klassieke implementatiemodel, dus veel rekenknooppunten implementeren kan enkele minuten duren. Om de RDMA-netwerkverbinding wanneer u het klassieke implementatiemodel gebruikt, implementeert u de virtuele machines in dezelfde cloudservice.
* **Azure Resource Manager-sjablonen**: U kunt ook het implementatiemodel van Resource Manager gebruiken voor het implementeren van een cluster met RDMA-compatibele virtuele machines die is verbonden met het netwerk RDMA. U kunt [uw eigen sjabloon maken](../../../resource-group-authoring-templates.md), of Raadpleeg de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor sjablonen die is bijgedragen door Microsoft of de community van de oplossing die u wilt implementeren. Resource Manager-sjablonen kunnen bieden een snelle en betrouwbare manier voor het implementeren van een Linux-cluster. Om de RDMA-netwerkverbinding wanneer u het implementatiemodel van Resource Manager gebruikt, implementeert u de virtuele machines in dezelfde beschikbaarheidsset.
* **HPC Pack**: een Microsoft HPC Pack-cluster in Azure maken en toevoegen van RDMA-compatibele rekenknooppunten waarop een ondersteunde Linux-distributie voor toegang tot de RDMA-netwerk wordt uitgevoerd. Zie voor meer informatie [aan de slag met Linux-rekenknooppunten in een cluster HPC Pack Azure](hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-the-classic-model"></a>Voorbeeld van implementatie van de stappen in het klassieke model
De volgende stappen laten zien hoe de Azure CLI gebruiken een SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM implementeren vanaf Azure Marketplace, aanpassen en het maken van een aangepaste VM-installatiekopie. Vervolgens kunt u de installatiekopie van het script voor de implementatie van een cluster met RDMA-compatibele virtuele machines.

> [!TIP]
> Gebruik van gelijksoortige stappen voor het implementeren van een cluster met RDMA-compatibele virtuele machines op basis van installatiekopieën op basis van CentOS HPC in Azure Marketplace. Een aantal stappen verschillen, zoals aangegeven. 
>
>

### <a name="prerequisites"></a>Vereisten
* **Clientcomputer**: U moet een Mac, Linux of Windows-clientcomputer om te communiceren met Azure. Deze stappen wordt ervan uitgegaan dat u een Linux-client.
* **Azure-abonnement**: als u geen een abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten. Voor grotere clusters kunt u een abonnement op gebruiksbasis of andere Aankoopopties.
* **Beschikbaarheid van de VM-grootte**: de volgende exemplaar grootten zijn RDMA-compatibele: H16r, H16mr A8 en A9. Controleer [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/) voor beschikbaarheid in een Azure-regio's.
* **Quotum voor kernen**: mogelijk moet u het quotum van kernen voor het implementeren van een cluster van virtuele machines rekenintensieve verhogen. Bijvoorbeeld, moet u ten minste 128 kernen als u implementeren 8 A9-VM's wilt, zoals in dit artikel. Uw abonnement mogelijk ook beperken het aantal kernen dat u in de grootte van families van bepaalde VM, waaronder de H-reeks kunt implementeren. Om aan te vragen een verhoging van het quotum [opent u een ondersteuningsaanvraag online klant](../../../azure-supportability/how-to-create-azure-support-request.md) zonder kosten.
* **Azure CLI**: [installeren](../../../cli-install-nodejs.md) de Azure CLI en [verbinding maken met uw Azure-abonnement](/cli/azure/authenticate-azure-cli) vanaf de clientcomputer.

### <a name="provision-an-sles-12-sp1-hpc-vm"></a>Een VM SLES 12 SP1 HPC inrichten
Na het aanmelden bij Azure met Azure CLI, voert `azure config list` om te bevestigen dat de uitvoer ziet u Service Management-modus. Als dit niet het geval is, stelt u de modus met deze opdracht:

    azure config mode asm


Typ het volgende als u wilt weergeven van alle abonnementen die u gemachtigd bent om te gebruiken:

    azure account list

Het huidige actieve abonnement wordt geïdentificeerd met `Current` ingesteld op `true`. Als dit abonnement niet degene die u wilt gebruiken om de cluster te maken, stelt u de juiste abonnements-ID als het actief abonnement:

    azure account set <subscription-Id>

Overzicht van de openbaar SLES 12 SP1 HPC-afbeeldingen in Azure, voert u een opdracht als volgt, ervan uitgaande dat uw omgeving shell ondersteunt **grep**:

    azure vm image list | grep "suse.*hpc"

Richt een RDMA-compatibele virtuele machine met een installatiekopie SLES 12 SP1 HPC door het uitvoeren van een opdracht als volgt:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Waar:

* De grootte (A9 in dit voorbeeld) is een van de RDMA-compatibele VM-grootte.
* De externe SSH-poortnummer (22 in dit voorbeeld de standaardinstelling SSH is) is een geldig poortnummer. Het interne SSH-poortnummer is ingesteld op 22.
* Een nieuwe cloudservice wordt gemaakt in de Azure-regio opgegeven door de locatie. Geef een locatie waarin de VM-grootte die u beschikbaar is.
* Voor ondersteuning van prioriteit SUSE (die extra kosten in rekening worden gebracht), de naam van de SLES 12 SP1-installatiekopie op dit moment mag een van deze twee opties: 

 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824`

  `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824`


### <a name="customize-the-vm"></a>De virtuele machine aanpassen
Nadat de virtuele machine is voltooid met het inrichten van SSH met de virtuele machine met behulp van de VM externe IP-adres (of DNS-naam) en de externe poort nummer dat u hebt geconfigureerd, en vervolgens aan te passen. Zie voor meer informatie verbinding [aanmelden met een virtuele machine met Linux](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Opdrachten uitvoeren als de gebruiker die u hebt geconfigureerd op de virtuele machine, tenzij de toegang tot de hoofdmap is vereist om een stap te voltooien.

> [!IMPORTANT]
> Microsoft Azure biedt geen toegang tot de hoofdmap voor virtuele Linux-machines. Beheerdersrechten om toegang te krijgen wanneer verbonden als een gebruiker voor de virtuele machine, kunt u opdrachten uitvoeren met behulp van `sudo`.
>
>

* **Updates**: Installeer updates met behulp van zypper. U kunt ook hulpprogramma's voor NFS te installeren.

  > [!IMPORTANT]
  > U wordt aangeraden dat u kernel-updates, die de oorzaak van problemen met het Linux-RDMA stuurprogramma's niet toe te passen in een VM SLES 12 SP1 HPC.
  >
  >
* **Intel MPI**: de installatie van Intel MPI op de virtuele machine SLES 12 SP1 HPC voltooid met de volgende opdracht:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
* **Vergrendelen van geheugen**: voor MPI-codes vergrendelen van het beschikbare geheugen voor RDMA, toevoegen of wijzigen van de volgende instellingen in het bestand /etc/security/limits.conf. U moet toegang tot de hoofdmap om dit bestand te bewerken.

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

  > [!NOTE]
  > Voor testdoeleinden kunt u ook memlock instellen op onbeperkt. Bijvoorbeeld: `<User or group name>    hard    memlock unlimited`. Zie voor meer informatie [aanbevolen bekende methoden voor instelling vergrendeld geheugengrootte](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).
  >
  >
* **SSH-sleutels voor virtuele machines SLES**: genereren SSH-sleutels vertrouwensrelatie voor uw gebruikersaccount tussen de rekenknooppunten in de SLES cluster bij het uitvoeren van MPI-taken. Als u een VM op basis van CentOS HPC hebt geïmplementeerd, niet volgt u deze stap. Zie de instructies verderop in dit artikel voor het instellen van passwordless SSH-vertrouwensrelatie tussen de clusterknooppunten, nadat u de installatiekopie vastlegt en implementeert het cluster.

    Voer de volgende opdracht voor het maken van SSH-sleutels. Wanneer u wordt gevraagd om invoer, selecteert u **Enter** voor het genereren van de sleutels in de standaardlocatie zonder een wachtwoord wordt ingesteld.

        ssh-keygen

    De openbare sleutel toevoegen aan het bestand authorized_keys voor bekende openbare sleutels.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    In de map ~/.ssh bewerken of maken van het configuratiebestand. Geef het IP-adresbereik van het particuliere netwerk die u wilt gebruiken in Azure (10.32.0.0/16 in dit voorbeeld):

        host 10.32.0.*
        StrictHostKeyChecking no

    U kunt ook een overzicht van het particuliere netwerk-IP-adres van elke virtuele machine in uw cluster als volgt:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

  > [!NOTE]
  > Configureren van `StrictHostKeyChecking no` een beveiligingsrisico met zich mee kunt maken wanneer er een specifiek IP-adres of -bereik is opgegeven.
  >
  >
* **Toepassingen**: installeert u alle toepassingen die u nodig hebt of andere aanpassingen uitvoeren voordat u de installatiekopie vastlegt.

### <a name="capture-the-image"></a>De installatiekopie vastleggen
Als u wilt de installatiekopie vastlegt, moet u eerst de volgende opdracht uitvoeren op de Linux-VM. Met deze opdracht deprovisions van de virtuele machine, maar behoudt gebruikersaccounts en SSH-sleutels die u hebt ingesteld.

```
sudo waagent -deprovision
```

Voer de volgende Azure CLI-opdrachten voor het vastleggen van de afbeelding vanaf de clientcomputer. Zie voor meer informatie [klassieke virtuele Linux-machine als afbeelding vastleggen](capture-image-classic.md).  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Nadat u deze opdrachten uitvoeren, de VM-installatiekopie wordt vastgelegd voor het gebruik en de virtuele machine is verwijderd. U hebt nu uw aangepaste installatiekopie gereed is voor het implementeren van een cluster.

### <a name="deploy-a-cluster-with-the-image"></a>Een cluster met de installatiekopie implementeren
Wijzig het volgende Bash-script met de juiste waarden voor uw omgeving en voer dit vanaf de clientcomputer. Omdat Azure de VM's in het klassieke implementatiemodel opeenvolgend implementeert, duurt het enkele minuten voor het implementeren van de acht A9-VM's in dit script wordt voorgesteld.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
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
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Overwegingen voor een CentOS HPC-cluster
Als u een op basis van een van de installatiekopieën op basis van CentOS HPC in Azure Marketplace in plaats van SLES 12 voor HPC-cluster instellen wilt, volgt u de algemene stappen in de vorige sectie. Let op de volgende verschillen wanneer u inrichten en de virtuele machine configureren:

- Intel MPI is al geïnstalleerd op een virtuele machine vanaf een installatiekopie van een HPC op basis van CentOS wordt ingericht.
- Vergrendeling geheugeninstellingen zijn al toegevoegd in de VM /etc/security/limits.conf-bestand.
- Genereren geen SSH-sleutels op de virtuele machine die u inricht voor vastleggen. In plaats daarvan wordt aangeraden verificatie op basis van een gebruiker instellen nadat u het cluster hebt geïmplementeerd. Zie de volgende sectie voor meer informatie.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Passwordless SSH vertrouwen op het cluster instellen
Op een op basis van CentOS HPC-cluster, er zijn twee methoden voor het instellen van een vertrouwensrelatie tussen de rekenknooppunten: verificatie op basis van de host en verificatie op basis van gebruiker. Verificatie op basis van de host is buiten het bereik van dit artikel en over het algemeen moet worden uitgevoerd via een script extensie tijdens de implementatie. Verificatie op basis van een gebruiker is handig voor het instellen van een vertrouwensrelatie na de implementatie en het genereren en delen van SSH-sleutels tussen de rekenknooppunten in het cluster is vereist. Deze methode staat bekend als passwordless SSH-aanmelding en is vereist bij het uitvoeren van MPI-taken.

Een voorbeeld van een script van de community bijgedragen is beschikbaar op [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) eenvoudig verificatie op een op basis van CentOS HPC-cluster in te schakelen. Download en gebruik dit script met behulp van de volgende stappen uit. U kunt ook dit script wijzigen of een andere methode gebruiken om passwordless SSH-verificatie tussen de clusterknooppunten compute stand te brengen.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh

Het script wordt uitgevoerd, moet u weten van het voorvoegsel voor uw subnet IP-adressen. Haal het voorvoegsel door de volgende opdracht in een van de clusterknooppunten worden uitgevoerd. De uitvoer ziet er ongeveer als 10.1.3.5 en het voorvoegsel zijn de/m 10.1.3 gedeelte.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Voer nu het script met behulp van drie parameters: de algemene gebruikersnaam op de rekenknooppunten, de algemene wachtwoord voor de gebruiker op de rekenknooppunten en het subnetvoorvoegsel die is geretourneerd van de vorige opdracht.

    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Dit script doet het volgende:

* Maakt een map op de hostknooppunt met SSH, de naam die is vereist voor passwordless aanmelding.
* Maakt een configuratiebestand in de SSH-map zodat passwordless aanmelden bij de aanmelding vanaf elk knooppunt in het cluster toestaan.
* Maakt bestanden die de knooppuntnamen en het knooppunt IP-adressen voor alle knooppunten in het cluster. Deze bestanden blijven nadat het script wordt uitgevoerd voor naslagdoeleinden.
* Hiermee maakt u een persoonlijke en openbare sleutelpaar voor elk clusterknooppunt (inclusief het hostknooppunt) en vermeldingen in het bestand authorized_keys maakt.

> [!WARNING]
> Dit script uitvoert, kan een beveiligingsrisico met zich mee maken. Zorg ervoor dat de gegevens van de openbare sleutel in ~/.ssh niet is gedistribueerd.
>
>

## <a name="configure-intel-mpi"></a>Intel MPI configureren
MPI-toepassingen op Azure Linux RDMA uitgevoerd, moet u bepaalde omgevingsvariabelen die specifiek zijn voor Intel MPI configureren. Hier volgt een voorbeeld Bash-script voor het configureren van de variabelen die nodig zijn voor het uitvoeren van een toepassing. Wijzig het pad in mpivars.sh zo nodig voor de installatie van Intel MPI.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

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

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

De indeling van de hostbestand is als volgt. Een regel voor elk knooppunt in het cluster toevoegen. Geef de privé IP-adressen van het virtuele netwerk eerder hebt gedefinieerd, niet de DNS-namen. Voor twee hosts met IP-adressen 10.32.0.1 en 10.32.0.2 bevat het bestand bijvoorbeeld het volgende:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>MPI worden uitgevoerd op een basic cluster met twee knooppunten
Als u dit nog niet hebt gedaan, eerst de omgeving instellen voor Intel MPI.

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-an-mpi-command"></a>Voer een MPI-opdracht
Een MPI-opdracht uitvoeren op een van de rekenknooppunten om weer te geven dat MPI juist is geïnstalleerd en communiceren kan tussen ten minste dat twee rekenknooppunten. De volgende **mpirun** opdracht wordt uitgevoerd de **hostnaam** opdracht op twee knooppunten.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
De uitvoer moet bevatten de namen van alle knooppunten die u hebt doorgegeven als invoer voor `-hosts`. Bijvoorbeeld, een **mpirun** opdracht met twee knooppunten retourneert de volgende uitvoer:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Voer een benchmark MPI
De volgende Intel MPI-opdracht wordt uitgevoerd een benchmark pingpong om te controleren of de configuratie van het cluster en de verbinding met het netwerk RDMA.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Op een werkende-cluster met twee knooppunten, moet u de volgende uitvoer weergegeven. Op het netwerk Azure RDMA verwachten latentie op of onder 3 microseconden voor het bericht de grootte van maximaal 512 bytes.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
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



## <a name="next-steps"></a>Volgende stappen
* Implementeren en uw Linux MPI-toepassingen uitvoeren op uw Linux-cluster.
* Zie de [Intel MPI-bibliotheek documentatie](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) voor hulp bij het Intel MPI.
* Probeer een [snelstartsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) voor het maken van een cluster Intel Lustre met behulp van een installatiekopie op basis van CentOS HPC. Zie voor meer informatie [Intel Cloud Edition implementeren voor Lustre in Microsoft Azure](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
