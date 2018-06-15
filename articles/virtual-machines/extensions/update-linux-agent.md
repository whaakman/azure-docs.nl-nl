---
title: Werk de Azure Linux Agent vanuit GitHub | Microsoft Docs
description: Informatie over het bijwerken van Azure Linux Agent voor uw Linux-VM in Azure
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: danis
ms.openlocfilehash: b5a482ef6d15f1c6b1942a6128a807d7c6189918
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33942800"
---
# <a name="how-to-update-the-azure-linux-agent-on-a-vm"></a>Het bijwerken van de Azure Linux Agent op een virtuele machine

Bijwerken van uw [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) op een Linux-VM in Azure, moet u al hebben:

- Een actieve Linux VM in Azure.
- Een verbinding met die Linux VM via SSH.

U moet altijd eerst in voor een pakket in de opslagplaats van Linux distro. Het is mogelijk dat het pakket beschikbaar zijn mogelijk niet de nieuwste versie, echter inschakelen autoupdate zorgt voor dat de Linux-Agent wordt altijd de nieuwste update downloaden. Hebt u problemen met het installeren van het pakket managers, moet u ondersteuning van de leverancier distro zoeken.

## <a name="minimum-virtual-machine-agent-support-in-azure"></a>Agent-ondersteuning voor minimale virtuele machine in Azure
Controleer of de [minimaal versie-ondersteuning voor agents van de virtuele machine in Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voordat u doorgaat.

## <a name="updating-the-azure-linux-agent"></a>De Azure Linux-Agent bijwerken

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Update-pakket-cache

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

#### <a name="restart-agent-for-1404"></a>Agent opnieuw starten om 14.04

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a>Agent opnieuw starten om 16.04 / 17.04

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a>Debian

### <a name="debian-7-wheezy"></a>Debian 7 'Wheezy'

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Update-pakket-cache

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Agent automatisch bijwerken inschakelen
Deze versie van Debian beschikt niet over een versie > = 2.0.16, automatisch bijwerken is daarom niet beschikbaar. De uitvoer van de bovenstaande opdracht ziet u als het pakket bijgewerkt is.

### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 'Jessie' / Debian 9 'Stretch'

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
apt list --installed | grep waagent
```

#### <a name="update-package-cache"></a>Update-pakket-cache

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a>RedHat / CentOS

### <a name="rhelcentos-6"></a>RHEL/CentOS 6

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Beschikbare updates controleren

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/\# AutoUpdate.Enabled=y/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a>RHEL/CentOS 7

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Beschikbare updates controleren

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a>SUSE SLES

### <a name="suse-sles-11-sp4"></a>SUSE SLES 11 SP4

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Beschikbare updates controleren

De bovenstaande uitvoer ziet u als het pakket bijgewerkt is.

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a>SUSE SLES 12 SP2

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Beschikbare updates controleren

In de uitvoer van de bovenstaande ziet hier u als het pakket maximaal datum is.

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a>Oracle 6 en 7

Voor Oracle Linux, Controleer of de `Addons` opslagplaats is ingeschakeld. Kies het bestand bewerken `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), en wijzigt u de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in deze het bestand.

Typ vervolgens de volgende opdracht voor het installeren van de nieuwste versie van de Azure Linux Agent:

```bash
sudo yum install WALinuxAgent
```

Als u de opslagplaats van de invoegtoepassing niet vinden kunt u eenvoudig deze regels toevoegen aan het einde van het bestand .repo volgens uw Oracle Linux-versie:

Voor Oracle Linux 6 virtuele machines:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Voor Oracle Linux 7 virtuele machines:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=1
```

Typ vervolgens:

```bash
sudo yum update WALinuxAgent
```

Dit is normaal alles wat u nodig hebt, maar als voor een bepaalde reden moet u Installeer deze via https://github.com rechtstreeks, gebruik de volgende stappen.


## <a name="update-the-linux-agent-when-no-agent-package-exists-for-distribution"></a>De Linux-Agent bijwerken wanneer er geen agentpakket voor distributie bestaat

Wget (Er zijn een aantal distributies dat niet het standaard, zoals Redhat en CentOS, Oracle Linux versies, 6.4 en 6.5) installeren door te typen `sudo yum install wget` op de opdrachtregel.

### <a name="1-download-the-latest-version"></a>1. Download de nieuwste versie
Open [de release van Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in een webpagina en lees het versienummer van de meest recente. (U kunt uw huidige versie vinden door te typen `waagent --version`.)

#### <a name="for-version-22x-or-later-type"></a>Voor versie 2.2.x of hoger, typ:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip
cd WALinuxAgent-2.2.x
```

De volgende regel wordt versie 2.2.0 als voorbeeld gebruikt:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-the-azure-linux-agent"></a>2. Azure Linux-Agent installeren

#### <a name="for-version-22x-use"></a>Voor versie 2.2.x, gebruiken:
Mogelijk moet u het pakket installeren `setuptools` eerst--Zie [hier](https://pypi.python.org/pypi/setuptools). Voer vervolgens:

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatische updates is ingeschakeld

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

'AutoUpdate.Enabled' vinden. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Voer inschakelen:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-the-waagent-service"></a>3. De waagent-service opnieuw starten
Voor het merendeel van Linux-distributies:

```bash
sudo service waagent restart
```

Ubuntu, gebruikt u in:

```bash
sudo service walinuxagent restart
```

Voor virtuele CoreOS, gebruiken:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-the-azure-linux-agent-version"></a>4. De versie van Azure Linux Agent bevestigen
    
```bash
waagent -version
```

Voor virtuele CoreOS, de bovenstaande opdracht werkt mogelijk niet.

U ziet dat de versie van Azure Linux Agent is bijgewerkt naar de nieuwe versie.

Zie voor meer informatie over de Azure Linux Agent [Azure Linux Agent Leesmij](https://github.com/Azure/WALinuxAgent).
