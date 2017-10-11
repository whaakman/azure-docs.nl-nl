---
title: Azure N-reeks stuurprogramma-instellingen voor Linux | Microsoft Docs
description: Het instellen van NVIDIA GPU-stuurprogramma's voor N-reeks virtuele machines waarop Linux wordt uitgevoerd in Azure
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d91695d0-64b9-4e6b-84bd-18401eaecdde
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bdeb4d5ca1d9ff4d7dfd0961690412dd7530572a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="install-nvidia-gpu-drivers-on-n-series-vms-running-linux"></a>NVIDIA GPU-stuurprogramma's installeren op N-reeks virtuele machines waarop Linux wordt uitgevoerd

Als u wilt profiteren van de GPU-mogelijkheden van N-reeks virtuele machines in Azure waarop Linux wordt uitgevoerd, ondersteunde NVIDIA graphics-stuurprogramma's te installeren. Dit artikel bevat de installatiestappen stuurprogramma nadat u een VM N-serie implementeren. Stuurprogramma-installatie-informatie is ook beschikbaar voor [VM's van Windows](../windows/n-series-driver-setup.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


Zie voor N-serie VM specificaties opslagcapaciteit en details van de schijf, [GPU Linux VM-grootten](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 



[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

## <a name="install-grid-drivers-for-nv-vms"></a>RASTER stuurprogramma's voor virtuele machines NV installeren

NVIDIA RASTER stuurprogramma's installeren op virtuele machines NV, een SSH-verbinding maken met elke VM en volg de stappen voor uw Linux-distributie. 

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
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
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


### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>Op basis van centOS 7.3 of Red Hat Enterprise Linux 7.3


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
 
3. De virtuele machine opnieuw opstarten, opnieuw verbinding maken en installeer de meest recente Linux-integratieservices voor Hyper-V:
 
  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
 
  tar xvzf lis-rpms-4.2.2-2.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
4. Opnieuw verbinding maken met de virtuele machine en voer de `lspci` opdracht. Controleer of de NVIDIA M60 kaart of kaarten zichtbaar als PCI-apparaten zijn.
 
5. Download en installeer het stuurprogramma RASTER:

  ```bash
  wget -O NVIDIA-Linux-x86_64-367.106-grid.run https://go.microsoft.com/fwlink/?linkid=849941  

  chmod +x NVIDIA-Linux-x86_64-367.106-grid.run

  sudo ./NVIDIA-Linux-x86_64-367.106-grid.run
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

Vergelijkbaar met de volgende uitvoer wordt weergegeven:

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
 
De BusID kunt u vinden door te voeren

```bash
/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1
```
 
De BusID kunt wijzigen als een virtuele machine opgehaald opnieuw toegewezen of opnieuw opgestart. Daarom kunt u een script gebruiken om bij te werken de BusID in de X11 configuratie wanneer u een virtuele machine opnieuw is opgestart. Bijvoorbeeld:

```bash 
#!/bin/bash
BUSID=$((16#`/usr/bin/nvidia-smi --query-gpu=pci.bus_id --format=csv | tail -1 | cut -d ':' -f 1`))

if grep -Fxq "${BUSID}" /etc/X11/XF86Config; then     echo "BUSID is matching"; else   echo "BUSID changed to ${BUSID}" && sed -i '/BusID/c\    BusID          \"PCI:0@'${BUSID}':0:0:0\"' /etc/X11/XF86Config; fi
```

Dit bestand kan worden aangeroepen als basis voor opgestart door het maken van een vermelding voor het in `/etc/rc.d/rc3.d`.


## <a name="install-cuda-drivers-for-nc-vms"></a>Voor virtuele machines NC CUDA stuurprogramma's installeren

Hier volgen de stappen NVIDIA-stuurprogramma's installeren op Linux NC virtuele machines van de NVIDIA CUDA Toolkit 8.0. 

C en C++-ontwikkelaars kunnen desgewenst installeren voor de volledige Toolkit om GPU-versnelde toepassingen te bouwen. Zie voor meer informatie de [CUDA installatiehandleiding](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html).


> [!NOTE]
> CUDA stuurprogramma downloadkoppelingen opgegeven op het moment van publicatie hier actueel zijn. Voor de nieuwste stuurprogramma's CUDA gaat u naar de [NVIDIA](http://www.nvidia.com/) website.
>

Zorg voor het installeren van CUDA Toolkit een SSH-verbinding naar elke virtuele machine. Om te controleren of het systeem een GPU die compatibel is met CUDA heeft, voer de volgende opdracht:

```bash
lspci | grep -i NVIDIA
```
Hier ziet u uitvoer die vergelijkbaar is met het volgende voorbeeld (met een kaart NVIDIA Tesla R80):

![opdrachtuitvoer lspci](./media/n-series-driver-setup/lspci.png)

Vervolgens uitvoeren installatieopdrachten die specifiek is voor uw distributiepunt.

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

1. Download en installeer de CUDA stuurprogramma's.
  ```bash
  CUDA_REPO_PKG=cuda-repo-ubuntu1604_8.0.61-1_amd64.deb

  wget -O /tmp/${CUDA_REPO_PKG} http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/${CUDA_REPO_PKG} 

  sudo dpkg -i /tmp/${CUDA_REPO_PKG}

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

### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>Op basis van centOS 7.3 of Red Hat Enterprise Linux 7.3

1. Updates downloaden. 

  ```bash
  sudo yum update

  sudo reboot
  ```
2. Opnieuw verbinding maken met de virtuele machine en installeer de meest recente Linux-integratieservices voor Hyper-V.

  > [!IMPORTANT]
  > Als u een installatiekopie op basis van CentOS HPC geïnstalleerd op een NC24r VM, gaat u verder met stap 3. Omdat Azure RDMA-stuurprogramma's en Linux-integratieservices vooraf worden geïnstalleerd in de installatiekopie zijn, LIS moet niet worden bijgewerkt en kernel-updates zijn standaard uitgeschakeld.
  >

  ```bash
  wget http://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.1.tar.gz
 
  tar xvzf lis-rpms-4.2.1.tar.gz
 
  cd LISISO
 
  sudo ./install.sh
 
  sudo reboot
  ```
 
3. Opnieuw verbinding maken met de virtuele machine en doorgaan met installatie met de volgende opdrachten:

  ```bash
  sudo yum install kernel-devel

  sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  sudo yum install dkms

  CUDA_REPO_PKG=cuda-repo-rhel7-8.0.61-1.x86_64.rpm

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

Vergelijkbaar met de volgende uitvoer wordt weergegeven:

![De apparaatstatus NVIDIA](./media/n-series-driver-setup/smi.png)


### <a name="cuda-driver-updates"></a>Updates voor stuurprogramma's CUDA

We bevelen aan dat u regelmatig CUDA stuurprogramma's na de implementatie.

#### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

```bash
sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get dist-upgrade -y

sudo apt-get install cuda-drivers

sudo reboot
```


#### <a name="centos-based-73-or-red-hat-enterprise-linux-73"></a>Op basis van centOS 7.3 of Red Hat Enterprise Linux 7.3

```bash
sudo yum update

sudo reboot
```



## <a name="troubleshooting"></a>Problemen oplossen

* Er is een bekend probleem met stuurprogramma's voor CUDA op N-reeks virtuele machines in Azure die de 4.4.0-75 Linux kernel op Ubuntu 16.04 TNS worden uitgevoerd. Als u een upgrade vanaf een eerdere kernelversie uitvoert, upgrade uit naar ten minste kernel versie 4.4.0-77. 



## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de NVIDIA GPU's op de N-reeks virtuele machines:
    * [NVIDIA Tesla R80](http://www.nvidia.com/object/tesla-k80.html) (voor NC Azure VM's)
    * [NVIDIA Tesla M60](http://www.nvidia.com/object/tesla-m60.html) (voor NV Azure VM's)

* Zie voor het vastleggen van een Linux VM-installatiekopie met de geïnstalleerde stuurprogramma's voor NVIDIA [generaliseren en Linux-machine vastleggen](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
