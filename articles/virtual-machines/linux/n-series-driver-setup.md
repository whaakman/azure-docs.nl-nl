---
title: Azure N-reeks stuurprogramma-instellingen voor Linux | Microsoft Docs
description: Het instellen van NVIDIA GPU-stuurprogramma's voor N-reeks virtuele machines waarop Linux wordt uitgevoerd in Azure
services: virtual-machines-linux
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/03/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f6b91f09b6c38c5461638b953f3a0df921fc7c30
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>NVIDIA GPU-stuurprogramma's installeren op N-reeks virtuele machines waarop Linux wordt uitgevoerd

Als u wilt profiteren van de GPU-mogelijkheden van N-reeks virtuele machines in Azure waarop Linux wordt uitgevoerd, moeten de NVIDIA grafische stuurprogramma's worden geïnstalleerd. Dit artikel bevat de installatiestappen stuurprogramma nadat u een VM N-serie implementeren. Stuurprogramma-installatie-informatie is ook beschikbaar voor [VM's van Windows](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zie voor N-serie VM specificaties opslagcapaciteit en details van de schijf, [GPU Linux VM-grootten](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-cuda-drivers-for-nc-ncv2-ncv3-and-nd-series-vms"></a>Voor NC, NCv2 NCv3 en ND-serie VMs CUDA stuurprogramma's installeren

Hier vindt u stappen voor het installeren van stuurprogramma's CUDA uit de werkset NVIDIA CUDA op virtuele machines N-serie. 


C en C++-ontwikkelaars kunnen desgewenst installeren voor de volledige Toolkit om GPU-versnelde toepassingen te bouwen. Zie voor meer informatie de [CUDA installatiehandleiding](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).

Zorg voor het installeren van stuurprogramma's CUDA, een SSH-verbinding naar elke virtuele machine. Om te controleren of het systeem een GPU die compatibel is met CUDA heeft, voer de volgende opdracht:

```bash
lspci | grep -i NVIDIA
```
Hier ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld (met een kaart NVIDIA Tesla R80):

![opdrachtuitvoer lspci](./media/n-series-driver-setup/lspci.png)

Vervolgens uitvoeren installatieopdrachten die specifiek is voor uw distributiepunt.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Download en installeer de CUDA stuurprogramma's.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_9.1.85-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

  sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub 

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo apt-get update

  sudo apt-get install cuda-drivers

  ```

  De installatie kan enkele minuten duren.

2. Wilt eventueel de volledige CUDA toolkit installeren, typt u:

  ```bash
  sudo apt-get install cuda
  ```

3. Start de virtuele machine op en gaat u verder met de installatie verifiëren.

#### <a name="cuda-driver-updates"></a>Updates voor stuurprogramma's CUDA

We bevelen aan dat u regelmatig CUDA stuurprogramma's na de implementatie.

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```

### <a name="centos-or-red-hat-enterprise-linux-73-or-74"></a>CentOS of Red Hat Enterprise Linux 7.3 of 7.4

1. Bijwerken van de kernel.

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

  CUDA_REPO_PKG=cuda-repo-rhel7-9.1.85-1.x86_64.rpm

  wget http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/${CUDA_REPO_PKG} -O /tmp/${CUDA_REPO_PKG}

  sudo rpm -ivh /tmp/${CUDA_REPO_PKG}

  rm -f /tmp/${CUDA_REPO_PKG}

  sudo yum install cuda-drivers
  ```

  De installatie kan enkele minuten duren. 

4. Wilt eventueel de volledige CUDA toolkit installeren, typt u:

  ```bash
  sudo yum install cuda
  ```

5. Start de virtuele machine op en gaat u verder met de installatie verifiëren.

### <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma

Query uitvoeren op de apparaatstatus GPU SSH naar de virtuele machine en voer de [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma met het stuurprogramma geïnstalleerd. 

Als het stuurprogramma is geïnstalleerd, ziet u uitvoer ziet er als volgt. Houd er rekening mee dat **GPU-Util** 0% ziet, tenzij u de werkbelasting van een GPU die momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat uw versie van stuurprogramma en GPU details afwijken van de namen weergegeven.

![De apparaatstatus NVIDIA](./media/n-series-driver-setup/smi.png)

## <a name="rdma-network-connectivity"></a>RDMA-netwerkverbinding

RDMA-netwerkverbinding kan worden ingeschakeld op virtuele machines RDMA-compatibele N-serie, zoals NC24r geïmplementeerd in de dezelfde beschikbaarheidsset of VM-schaalset bevatten. Het netwerk RDMA ondersteunt Message Passing Interface (MPI)-verkeer voor toepassingen die worden uitgevoerd met Intel MPI 5.x of een latere versie. Aanvullende vereisten als volgt:

### <a name="distributions"></a>Distributies

RDMA-compatibele N-reeks-virtuele machines van een van de afbeeldingen in de Azure Marketplace die ondersteuning biedt voor RDMA-connectiviteit op N-reeks virtuele machines implementeren:
  
* **Ubuntu 16.04 LTS** - RDMA-stuurprogramma's op de virtuele machine configureren en registreren met Intel Intel MPI downloaden:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../../includes/virtual-machines-common-ubuntu-rdma.md)]

* **7.4 HPC op basis van centOS** -RDMA-stuurprogramma's en Intel MPI 5.1 zijn geïnstalleerd op de virtuele machine.

## <a name="install-grid-drivers-for-nv-series-vms"></a>RASTER stuurprogramma's installeren voor virtuele machines NV-serie

Een SSH-verbinding maken met elke VM wilt NVIDIA RASTER stuurprogramma's installeren op virtuele machines NV-serie, en volg de stappen voor uw Linux-distributie. 

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Voer de `lspci` opdracht. Controleer of de NVIDIA M60 kaart of kaarten zichtbaar als PCI-apparaten zijn.

2. Updates installeren.

  ```bash
  sudo apt-get update

  sudo apt-get upgrade -y

  sudo apt-get dist-upgrade -y

  sudo apt-get install build-essential ubuntu-desktop -y
  ```
3. Het stuurprogramma van de kernel Nouveau, dat niet compatibel met het stuurprogramma NVIDIA is uitschakelen. (Alleen NVIDIA stuurprogramma gebruiken op virtuele machines NV.) Om dit te doen, maakt u een bestand in `/etc/modprobe.d `met de naam `nouveau.conf` met de volgende inhoud:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```


4. Start de virtuele machine en opnieuw verbinding maken. Exit X-server:

  ```bash
  sudo systemctl stop lightdm.service
  ```

5. Download en installeer het stuurprogramma RASTER:

  ```bash
  wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-grid.run

  sudo ./NVIDIA-Linux-x86_64-grid.run
  ``` 

6. Wanneer u wordt gevraagd of u wilt uitvoeren van het hulpprogramma nvidia xconfig selecteren om bij te werken uw configuratiebestand X **Ja**.

7. Nadat de installatie is voltooid, /etc/nvidia/gridd.conf.template naar een nieuw bestand gridd.conf op locatie/etc/nvidia/kopiëren

  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```

8. Het volgende toevoegen aan `/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Start de virtuele machine op en gaat u verder met de installatie verifiëren.


### <a name="centos-or-red-hat-enterprise-linux"></a>CentOS of Red Hat Enterprise Linux 

1. De kernel en DKMS bijwerken.
 
  ```bash  
  sudo yum update
 
  sudo yum install kernel-devel
 
  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
  sudo yum install dkms
  ```

2. Het stuurprogramma van de kernel Nouveau, dat niet compatibel met het stuurprogramma NVIDIA is uitschakelen. (Alleen NVIDIA stuurprogramma gebruiken op virtuele machines NV.) Om dit te doen, maakt u een bestand in `/etc/modprobe.d `met de naam `nouveau.conf` met de volgende inhoud:

  ```
  blacklist nouveau

  blacklist lbm-nouveau
  ```
 
3. De virtuele machine opnieuw opstarten, opnieuw verbinding maken en installeer de meest recente [Linux-integratieservices voor Hyper-V en Azure](https://www.microsoft.com/download/details.aspx?id=55106).
 
  ```bash
  wget https://aka.ms/lis

  tar xvzf lis

  cd LISISO

  sudo ./install.sh

  sudo reboot

  ```
 
4. Opnieuw verbinding maken met de virtuele machine en voer de `lspci` opdracht. Controleer of de NVIDIA M60 kaart of kaarten zichtbaar als PCI-apparaten zijn.
 
5. Download en installeer het stuurprogramma RASTER:

  ```bash
  wget -O NVIDIA-Linux-x86_64-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-grid.run

  sudo ./NVIDIA-Linux-x86_64-grid.run
  ``` 
6. Wanneer u wordt gevraagd of u wilt uitvoeren van het hulpprogramma nvidia xconfig selecteren om bij te werken uw configuratiebestand X **Ja**.

7. Nadat de installatie is voltooid, /etc/nvidia/gridd.conf.template naar een nieuw bestand gridd.conf op locatie/etc/nvidia/kopiëren
  
  ```bash
  sudo cp /etc/nvidia/gridd.conf.template /etc/nvidia/gridd.conf
  ```
  
8. Het volgende toevoegen aan `/etc/nvidia/gridd.conf`:
 
  ```
  IgnoreSP=TRUE
  ```
9. Start de virtuele machine op en gaat u verder met de installatie verifiëren.

### <a name="verify-driver-installation"></a>Controleer of de installatie van stuurprogramma


Query uitvoeren op de apparaatstatus GPU SSH naar de virtuele machine en voer de [nvidia smi](https://developer.nvidia.com/nvidia-system-management-interface) opdrachtregelprogramma met het stuurprogramma geïnstalleerd. 

Als het stuurprogramma is geïnstalleerd, ziet u uitvoer ziet er als volgt. Houd er rekening mee dat **GPU-Util** 0% ziet, tenzij u de werkbelasting van een GPU die momenteel worden uitgevoerd op de virtuele machine. Het is mogelijk dat uw versie van stuurprogramma en GPU details afwijken van de namen weergegeven.

![De apparaatstatus NVIDIA](./media/n-series-driver-setup/smi-nv.png)
 

### <a name="x11-server"></a>X11 server
Als u een X11 moet server voor externe verbindingen met een VM NV [x11vnc](http://www.karlrunge.com/x11vnc/) wordt aanbevolen, omdat hiermee hardwareversnelling van afbeeldingen. De BusID van het apparaat M60 moet handmatig worden toegevoegd aan het bestand xconfig (`etc/X11/xorg.conf` op Ubuntu 16.04 TNS, `/etc/X11/XF86config` op CentOS 7.3 of Red Hat Enterprise Server 7.3). Voeg een `"Device"` vergelijkbaar met de volgende sectie:
 
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "Tesla M60"
    BusID          "your-BusID:0:0:0"
EndSection
```
 
Daarnaast uw `"Screen"` sectie voor het gebruik van dit apparaat.
 
Het decimaalteken BusID kunt u vinden door te voeren

```bash
echo $((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))
```
 
De BusID kunt wijzigen als een virtuele machine opgehaald opnieuw toegewezen of opnieuw opgestart. Daarom kunt u een script gebruiken om bij te werken de BusID in de X11 configuratie wanneer u een virtuele machine opnieuw is opgestart. Bijvoorbeeld:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed to ${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Dit bestand kan worden aangeroepen als basis voor opgestart door het maken van een vermelding voor het in `/etc/rc.d/rc3.d`.

## <a name="troubleshooting"></a>Problemen oplossen

* U kunt instellen persistentie modus met `nvidia-smi` zodat de uitvoer van de opdracht sneller is als u query-kaarten wilt. Persistentie als modus wilt instellen, uitvoeren `nvidia-smi -pm 1`. Houd er rekening mee dat als de virtuele machine opnieuw wordt opgestart, de instelling voor de verdwijnt. U kunt de instelling voor de uit te voeren bij het opstarten altijd script.

## <a name="next-steps"></a>Volgende stappen

* Zie voor het vastleggen van een Linux VM-installatiekopie met de geïnstalleerde stuurprogramma's voor NVIDIA [generaliseren en Linux-machine vastleggen](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
