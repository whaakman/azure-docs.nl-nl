---
title: Azure N-serie GPU-stuurprogramma-instellingen voor Linux | Microsoft Docs
description: Over het instellen van NVIDIA GPU-stuurprogramma's voor N-serie VM's waarop Linux wordt uitgevoerd in Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7c80b821d6bd0263473ba0178eea148f7a2d5773
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879043"
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>NVIDIA GPU-stuurprogramma's installeren op N-serie VM's waarop Linux wordt uitgevoerd

Als u wilt profiteren van de GPU-mogelijkheden van Azure VM's voor N-serie waarop Linux wordt uitgevoerd, moeten de NVIDIA GPU-stuurprogramma's worden geïnstalleerd. De [NVIDIA GPU-stuurprogramma extensie](../extensions/hpccompute-gpu-linux.md) juiste NVIDIA CUDA- of GRID stuurprogramma's worden geïnstalleerd op een VM uit de N-serie. Installeren of beheren van de extensie met de Azure portal of hulpprogramma's, zoals de Azure CLI of Azure Resource Manager-sjablonen. Zie de [NVIDIA GPU-stuurprogramma extensie documentatie](../extensions/hpccompute-gpu-linux.md) voor ondersteunde distributies en de implementatiestappen.

Als u ervoor kiest GPU-stuurprogramma's handmatig te installeren, wordt dit artikel bevat ondersteunde distributies en stuurprogramma's voor installatie-en verificatiestappen. Stuurprogramma voor handmatige installatie-informatie is ook beschikbaar voor [Windows VMs](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor een N-serie VM-specificaties, opslagcapaciteit en details over de schijf, [GPU Linux VM-grootten](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-cuda-drivers-on-n-series-vms"></a>CUDA-stuurprogramma's installeren op virtuele machines uit de N-serie

Hier vindt u instructies voor het installeren van CUDA-stuurprogramma's uit de werkset NVIDIA CUDA op N-serie VM's. 


C en C++-ontwikkelaars kunnen de volledige Toolkit om GPU-versnelde toepassingen te bouwen (optioneel) installeren. Zie voor meer informatie de [CUDA-installatiehandleiding](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

Voor het installeren van stuurprogramma's voor CUDA, maakt u een SSH-verbinding met elke virtuele machine. Als u wilt controleren of het systeem heeft een CUDA-compatibele GPU, voer de volgende opdracht:

```bash
lspci | grep -i NVIDIA
```
Hier ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld (met een NVIDIA Tesla R80-kaart):

![uitvoer van de opdracht lspci](./media/n-series-driver-setup/lspci.png)

Vervolgens uitvoeren installatieopdrachten die specifiek zijn voor uw distributie.

### <a name="ubuntu"></a>Ubuntu 

1. Download en installeer de CUDA-stuurprogramma's van de NVIDIA-website. Bijvoorbeeld: voor Ubuntu 16.04 LTS:
   ```bash
   CUDA_REPO_PKG=cuda-repo-ubuntu1604_10.0.130-1_amd64.deb

   wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

   sudo dpkg -i /tmp/${CUDA_REPO_PKG}

   sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub 

   rm -f /tmp/${CUDA_REPO_PKG}

   sudo apt-get update

   sudo apt-get install cuda-drivers

   ```

   De installatie kan enkele minuten duren.

2. Als u wilt installeren (optioneel) de volledige CUDA-toolkit, typt u:

   ```bash
   sudo apt-get install cuda
   ```

3. Start de VM opnieuw op en gaat u verder met de installatie controleren.

#### <a name="cuda-driver-updates"></a>CUDA-updates voor stuurprogramma 's

We bevelen aan dat u regelmatig CUDA-stuurprogramma's na de implementatie.

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```

### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS of Red Hat Enterprise Linux

1. Bijwerken de kernel (aanbevolen). Als u ervoor kiest om niet bij te werken van de kernel, zorg ervoor dat de versies van `kernel-devel` en `dkms` geschikt zijn voor de kernel.

   ```
   sudo yum install kernel kernel-tools kernel-headers kernel-devel
  
   sudo reboot

2. Install the latest [Linux Integration Services for Hyper-V and Azure](https://www.microsoft.com/download/details.aspx?id=55106).

   ```bash
   wget https://aka.ms/lis
 
   tar xvzf lis
 
   cd LISISO
 
   sudo ./install.sh
 
   sudo reboot
   ```
 
3. Opnieuw verbinding maken met de virtuele machine en doorgaan met installatie met de volgende opdrachten:

   ```bash
   sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

   sudo yum install dkms

   CUDA_REPO_PKG=cuda-repo-rhel7-10.0.130-1.x86_64.rpm

   wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

   sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

   rm -f /tmp/${CUDA_REPO_PKG}

   sudo yum install cuda-drivers
   ```

   De installatie kan enkele minuten duren. 

4. Als u wilt installeren (optioneel) de volledige CUDA-toolkit, typt u:

   ```bash
   sudo yum install cuda
   ```

5. Start de VM opnieuw op en gaat u verder met de installatie controleren.

### <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma

Query uitvoeren op de apparaatstatus GPU SSH naar de VM en voer de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma moet zijn geïnstalleerd met het stuurprogramma. 

Als het stuurprogramma is geïnstalleerd, ziet u uitvoer die vergelijkbaar is met het volgende. Houd er rekening mee dat **GPU-Util** 0% bevat, tenzij u een werkbelasting GPU momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat de stuurprogrammaversie en -details van GPU afwijken van de namen weergegeven.

![NVIDIA-Apparaatstatus](./media/n-series-driver-setup/smi.png)

## <a name="rdma-network-connectivity"></a>RDMA-netwerkverbinding

RDMA-netwerkverbinding kan worden ingeschakeld op RDMA-compatibele virtuele machines voor N-serie, zoals NC24r geïmplementeerd in dezelfde beschikbaarheidsset bevinden of in één plaatsingsgroep in een VM-schaalset. De RDMA-netwerk ondersteunt Message Passing Interface (MPI)-verkeer voor toepassingen die worden uitgevoerd met Intel MPI 5.x of een latere versie. Aanvullende vereisten als volgt:

### <a name="distributions"></a>Distributies

RDMA-compatibele N-serie VM's implementeren vanaf een van de installatiekopieën in de Azure Marketplace die ondersteuning biedt voor RDMA verbinding op N-serie VM's:
  
* **Ubuntu 16.04 LTS** - stuurprogramma's voor RDMA op de virtuele machine configureren en registreren met de Intel Intel MPI downloaden:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **Op basis van centOS 7.4 HPC** -stuurprogramma's voor RDMA en Intel MPI 5.1 zijn geïnstalleerd op de virtuele machine.

## <a name="install-grid-drivers-on-nv-or-nvv2-series-vms"></a>GRID-stuurprogramma's installeren op NV of NVv2-serie VM 's

Een SSH-verbinding maken met elke virtuele machine voor het installeren van stuurprogramma's van NVIDIA GRID op NV of NVv2-serie VM's, en volg de stappen voor uw Linux-distributie. 

### <a name="ubuntu"></a>Ubuntu 

1. Voer de opdracht `lspci` uit. Controleer of de NVIDIA M60-kaart of kaarten zichtbaar als PCI-apparaten zijn.

2. Updates installeren.

   ```bash
   sudo apt-get update

   sudo apt-get upgrade -y

   sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Het uitschakelen van het stuurprogramma van de kernel Nouveau, dit incompatibel met het NVIDIA-stuurprogramma is. (Gebruik alleen het NVIDIA-stuurprogramma op NV of NVv2 VM's.) Om dit te doen, maakt u een bestand in `/etc/modprobe.d` met de naam `nouveau.conf` met de volgende inhoud:

   ```
   blacklist nouveau

   blacklist lbm-nouveau
   ```


4. Opnieuw opstarten van de virtuele machine en maak opnieuw verbinding. Afsluiten X-server:

   ```bash
   sudo systemctl stop lightdm.service
   ```

5. Download en installeer het stuurprogramma GRID:

   ```bash
   wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=874272  

   chmod +x NVIDIA-Linux-x86_64-grid.run

   sudo ./NVIDIA-Linux-x86_64-grid.run
   ``` 

6. Wanneer u wordt gevraagd of u wilt uitvoeren van het hulpprogramma nvidia-xconfig selecteren om bij te werken uw configuratiebestand X **Ja**.

7. Nadat de installatie is voltooid, /etc/nvidia/gridd.conf.template naar een nieuw bestand gridd.conf op locatie/etc/nvidia/kopiëren

   ```bash
   sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
   ```

8. Het volgende toevoegen aan `/etc/nvidia/gridd.conf`:
 
   ```
   IgnoreSP=FALSE
   ```
9. Start de VM opnieuw op en gaat u verder met de installatie controleren.


### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS of Red Hat Enterprise Linux 

1. Werk de kernel en DKMS (aanbevolen). Als u ervoor kiest om niet bij te werken van de kernel, zorg ervoor dat de versies van `kernel-devel` en `dkms` geschikt zijn voor de kernel.
 
   ```bash  
   sudo yum update
 
   sudo yum install kernel-devel
 
   sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
   sudo yum install dkms
   ```

2. Het uitschakelen van het stuurprogramma van de kernel Nouveau, dit incompatibel met het NVIDIA-stuurprogramma is. (Gebruik alleen het NVIDIA-stuurprogramma op NV of NV2 VM's.) Om dit te doen, maakt u een bestand in `/etc/modprobe.d` met de naam `nouveau.conf` met de volgende inhoud:

   ```
   blacklist nouveau

   blacklist lbm-nouveau
   ```
 
3. De virtuele machine opnieuw opstarten, opnieuw verbinding maken en installeer de meest recente [Linux Integration Services for Hyper-V en Azure](https://www.microsoft.com/download/details.aspx?id=55106).
 
   ```bash
   wget https://aka.ms/lis

   tar xvzf lis

   cd LISISO

   sudo ./install.sh

   sudo reboot

   ```
 
4. Opnieuw verbinding maken met de VM en voer de `lspci` opdracht. Controleer of de NVIDIA M60-kaart of kaarten zichtbaar als PCI-apparaten zijn.
 
5. Download en installeer het stuurprogramma GRID:

   ```bash
   wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=874272  

   chmod +x NVIDIA-Linux-x86_64-grid.run

   sudo ./NVIDIA-Linux-x86_64-grid.run
   ``` 
6. Wanneer u wordt gevraagd of u wilt uitvoeren van het hulpprogramma nvidia-xconfig selecteren om bij te werken uw configuratiebestand X **Ja**.

7. Nadat de installatie is voltooid, /etc/nvidia/gridd.conf.template naar een nieuw bestand gridd.conf op locatie/etc/nvidia/kopiëren
  
   ```bash
   sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
   ```
  
8. Het volgende toevoegen aan `/etc/nvidia/gridd.conf`:
 
   ```
   IgnoreSP=FALSE
   ```
9. Start de VM opnieuw op en gaat u verder met de installatie controleren.

### <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma


Query uitvoeren op de apparaatstatus GPU SSH naar de VM en voer de [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma moet zijn geïnstalleerd met het stuurprogramma. 

Als het stuurprogramma is geïnstalleerd, ziet u uitvoer die vergelijkbaar is met het volgende. Houd er rekening mee dat **GPU-Util** 0% bevat, tenzij u een werkbelasting GPU momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat de stuurprogrammaversie en -details van GPU afwijken van de namen weergegeven.

![NVIDIA-Apparaatstatus](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 server
Als u een X11-server voor externe verbindingen met een NV of NVv2 VM, [x11vnc](http://www.karlrunge.com/x11vnc/) wordt aanbevolen, omdat hiermee hardwareversnelling van afbeeldingen. De BusID van het apparaat M60 moet handmatig worden toegevoegd aan de X11 configuratiebestand (meestal `etc/X11/xorg.conf`). Voeg een `"Device"` die vergelijkbaar is met de volgende sectie:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "PCI:0@your-BusID:0:0"
EndSection
```
 
Daarnaast uw `"Screen"` sectie om dit apparaat te gebruiken.
 
Het decimaalteken BusID kunt u vinden door te voeren

```bash
nvidia-xconfig --query-gpu-info | awk '/PCI BusID/{print $4}'
```
 
De BusID kunt wijzigen wanneer een virtuele machine wordt opnieuw toegewezen of opnieuw opgestart. Daarom kunt u het maken van een script voor het bijwerken van de BusID in de X11 configuratie als een virtuele machine opnieuw wordt opgestart. Maak bijvoorbeeld een script met de naam `busidupdate.sh` (of een andere naam die u kiest) met inhoud die vergelijkbaar is met het volgende:

```bash 
#!/bin/bash
XCONFIG="/etc/X11/xorg.conf"
OLDBUSID=`awk '/BusID/{gsub(/"/, "", $2); print $2}' ${XCONFIG}`
NEWBUSID=`nvidia-xconfig --query-gpu-info | awk '/PCI BusID/{print $4}'`

if [[ "${OLDBUSID}" == "${NEWBUSID}" ]] ; then
        echo "NVIDIA BUSID not changed - nothing to do"
else
        echo "NVIDIA BUSID changed from \"${OLDBUSID}\" to \"${NEWBUSID}\": Updating ${XCONFIG}" 
        sed -e 's|BusID.*|BusID          '\"${NEWBUSID}\"'|' -i ${XCONFIG}
fi
```

Vervolgens maakt u een vermelding voor uw updatescript in `/etc/rc.d/rc3.d` , zodat het script wordt aangeroepen als basis bij het opstarten.

## <a name="troubleshooting"></a>Problemen oplossen

* U kunt instellen persistentie modus met `nvidia-smi` , zodat de uitvoer van de opdracht sneller is wanneer u nodig met query-kaarten hebt. Uitvoeren om in te stellen de gebruikte modus, `nvidia-smi -pm 1`. Houd er rekening mee dat als de virtuele machine opnieuw wordt opgestart, de instelling voor de verdwenen. U kunt altijd de instelling uit te voeren bij het opstarten een script.

## <a name="next-steps"></a>Volgende stappen

* Zie voor het vastleggen van een Linux VM-installatiekopie met de geïnstalleerde stuurprogramma's van NVIDIA [het generaliseren en Linux-machine vastleggen](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
