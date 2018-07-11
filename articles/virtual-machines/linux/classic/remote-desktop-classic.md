---
title: Met een virtuele Linux-machine via Extern bureaublad | Microsoft Docs
description: Meer informatie over het installeren en configureren van extern bureaublad verbinding maken met een Microsoft Azure Linux-VM voor het klassieke implementatiemodel
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 5e68774c3edb7d82fef388c593a6b96c52857be6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927737"
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Extern bureaublad gebruiken om verbinding te maken met een Microsoft Azure Linux VM
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor de bijgewerkte Resource Manager-versie van dit artikel, [hier](../use-remote-desktop.md).

## <a name="overview"></a>Overzicht
RDP (Remote Desktop Protocol) is een eigen protocol gebruikt voor Windows. Hoe kunnen we RDP verbinding maken met een Linux-VM (virtuele machine) op afstand gebruiken?

Deze handleiding geeft u het antwoord. Het helpt u om te installeren en config-xrdp op uw Microsoft Azure Linux VM, waarmee u verbinding maken met extern bureaublad van een Windows-machine. Linux-VM met Ubuntu of OpenSUSE als in het voorbeeld in deze handleiding worden gebruikt.

Het hulpprogramma xrdp is een open-source RDP-server waarmee u verbinding maken met uw Linux-server met extern bureaublad van een Windows-machine. RDP is betere prestaties dan VNC (virtueel netwerk Computing). VNC rendert met hoogwaardige JPEG-afbeeldingen en kan traag zijn, terwijl RDP snel en crystal wissen is.

> [!NOTE]
> U moet al een Microsoft Azure-VM waarop Linux wordt uitgevoerd. Als u wilt maken en instellen van een Linux-VM, Zie de [zelfstudie voor Azure Linux VM](createportal-classic.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Een eindpunt maken voor extern bureaublad
We gebruiken het standaardeindpunt 3389 voor extern bureaublad in dit document. Instellen van 3389 eindpunt als `Remote Desktop` voor uw Linux-VM, zoals hieronder:

![installatiekopie](./media/remote-desktop/endpoint-for-linux-server.png)

Als u niet hoe u een eindpunt voor uw virtuele machine instelt weet, raadpleegt u [deze richtlijnen](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Gnome Desktop installeren
Verbinding maken met uw Linux-VM via `putty`, en installeer `Gnome Desktop`.

Gebruik voor Ubuntu:

```bash
sudo apt-get update
sudo apt-get install ubuntu-desktop
```

Voor OpenSUSE, gebruikt:

```bash
sudo zypper install gnome-session
```

## <a name="install-xrdp"></a>Xrdp installeren
Gebruik voor Ubuntu:

```bash
sudo apt-get install xrdp
```

Voor OpenSUSE, gebruikt:

> [!NOTE]
> Werk de OpenSUSE-versie met de versie die u gebruikt in de volgende opdracht uit. Het onderstaande voorbeeld is voor `OpenSUSE 13.2`.
> 
> 

```bash
sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc
```

## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Start xrdp en xdrp service zo instellen dat bij de keer opstarten
Voor OpenSUSE, gebruikt:

```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

Voor Ubuntu, worden xrdp gestart en ingeschakeld bij de keer opstarten automatisch na de installatie.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Met behulp van xfce als u een Ubuntu-versie hoger is dan Ubuntu 12.04LTS
Omdat de huidige versie van xrdp niet Gnome Desktop voor Ubuntu-versies hoger is dan Ubuntu 12.04LTS ondersteunt, gebruiken we `xfce` bureaublad in plaats daarvan.

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

Voeg de regel `xfce4-session` vóór de regel `/etc/X11/Xsession`.

Als u wilt de xrdp-service opnieuw start, gebruikt u dit:

```bash
sudo service xrdp restart
```

## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Verbind uw Linux-VM vanaf een Windows-machine
In een Windows-machine, start de extern bureaublad-client en voer de naam van uw DNS-Linux-VM. Of Ga naar het Dashboard van uw virtuele machine in Azure portal en klik op `Connect` om uw Linux-VM verbinding te maken. In dat geval ziet u het aanmeldingsvenster:

![installatiekopie](./media/remote-desktop/no2.png)

Meld u aan met de gebruikersnaam en wachtwoord van uw Linux-VM.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het gebruik van xrdp [ http://www.xrdp.org/ ](http://www.xrdp.org/).
