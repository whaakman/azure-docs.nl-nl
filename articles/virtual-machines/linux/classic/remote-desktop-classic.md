---
title: Extern bureaublad voor een virtuele Linux-machine | Microsoft Docs
description: Meer informatie over het installeren en configureren van extern bureaublad verbinding maken met een Microsoft Azure Linux VM voor het klassieke implementatiemodel
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: mingzhan
ms.openlocfilehash: 72c814aece7626b19cefccb18e1b90f8c44d7f57
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Extern bureaublad gebruiken om verbinding te maken met een Microsoft Azure Linux VM
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor de bijgewerkte Resource Manager-versie van dit artikel, [hier](../use-remote-desktop.md).

## <a name="overview"></a>Overzicht
RDP (Remote Desktop Protocol) is een oorspronkelijk protocol gebruikt voor Windows. Hoe kunnen we RDP extern verbinding kunnen maken met een Linux-VM (virtuele machine) gebruiken

In deze richtlijnen bieden u het antwoord! Het helpt u om te installeren en config-xrdp op uw Microsoft Azure Linux VM, waarmee u verbinding maken met extern bureaublad van een Windows-machine. Linux-VM met Ubuntu of OpenSUSE als in het voorbeeld in deze richtlijnen zullen worden gebruikt.

Het hulpprogramma xrdp is een open-source RDP-server waarmee u verbinding maken met de Linux-server met extern bureaublad van een Windows-machine. RDP heeft betere prestaties dan VNC (virtuele netwerk computers). VNC renders met JPEG-kwaliteit afbeeldingen en kan traag zijn, terwijl RDP snel en crystal wissen is.

> [!NOTE]
> U moet al een Microsoft Azure-virtuele machine met Linux hebben. Als u wilt maken en een Linux-VM instellen, Zie de [Azure Linux VM-zelfstudie](createportal-classic.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Een eindpunt voor extern bureaublad maken
We gebruiken het standaardeindpunt 3389 voor extern bureaublad in dit document. Instellen van 3389 eindpunt als `Remote Desktop` voor uw Linux-VM zoals hieronder:

![Afbeelding](./media/remote-desktop/endpoint-for-linux-server.png)

Als u niet hoe u een eindpunt voor de virtuele machine instelt weet, Zie [in deze richtlijnen](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Gnome bureaublad installeren
Verbinding maken met uw Linux-VM via `putty`, en installeer `Gnome Desktop`.

Ubuntu, gebruikt u in:

```bash
sudo apt-get update
sudo apt-get install ubuntu-desktop
```

Voor OpenSUSE, gebruiken:

```bash
sudo zypper install gnome-session
```

## <a name="install-xrdp"></a>Xrdp installeren
Ubuntu, gebruikt u in:

```bash
sudo apt-get install xrdp
```

Voor OpenSUSE, gebruiken:

> [!NOTE]
> De versie van de OpenSUSE bijwerken met de versie die u in de volgende opdracht gebruikt. Het onderstaande voorbeeld is voor `OpenSUSE 13.2`.
> 
> 

```bash
sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc
```

## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Start xrdp en xdrp service ingesteld op opstartprocedure
Voor OpenSUSE, gebruiken:

```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

Voor Ubuntu, wordt xrdp gestart en ingeschakeld op opstartprocedure automatisch na de installatie.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Met behulp van xfce als u een virtuele Ubuntu-versie hoger is dan Ubuntu 12.04LTS
Omdat de huidige versie van xrdp niet Gnome bureaublad voor Ubuntu-versies hoger is dan Ubuntu 12.04LTS ondersteunt, gebruiken we `xfce` bureaublad in plaats daarvan.

Voor het installeren van `xfce`, gebruikt u deze opdracht:

```bash
sudo apt-get install xubuntu-desktop
```

Schakel vervolgens `xfce` met de volgende opdracht:

```bash
echo xfce4-session >~/.xsession
```

Het configuratiebestand bewerken `/etc/xrdp/startwm.sh`:

```bash
sudo vi /etc/xrdp/startwm.sh   
```

Voeg de regel `xfce4-session` voor de regel `/etc/X11/Xsession`.

Als u wilt de xrdp-service opnieuw start, gebruiken deze:

```bash
sudo service xrdp restart
```

## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Verbinding maken met uw Linux-VM van een Windows-computer
Start de extern bureaublad-client in een Windows-computer en voer de naam van uw DNS-Linux-VM. Of Ga naar het Dashboard van de virtuele machine in de Azure-portal en klik op `Connect` verbinding maken uw Linux-VM. In dat geval ziet u het aanmeldingsvenster:

![Afbeelding](./media/remote-desktop/no2.png)

Aanmelden met de gebruikersnaam en wachtwoord van uw Linux-VM.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van xrdp [http://www.xrdp.org/](http://www.xrdp.org/).
