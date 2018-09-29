---
title: Werk de Azure Linux-Agent vanuit GitHub | Microsoft Docs
description: Meer informatie over het Azure Linux Agent bijwerken voor uw Linux-VM in Azure
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
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
ms.author: roiyz
ms.openlocfilehash: 0926859037c358aaf94ec51d9614cc9c02629e7f
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451932"
---
# <a name="how-to-update-the-azure-linux-agent-on-a-vm"></a>Het bijwerken van de Azure Linux Agent op een virtuele machine

Om bij te werken uw [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) op een Linux-VM in Azure, moet u al hebben:

- Een actieve Linux VM in Azure.
- Een verbinding met die Linux-VM met behulp van SSH.

U moet altijd eerst voor een pakket in de opslagplaats voor Linux-distributie. Het is mogelijk dat het pakket beschikbaar zijn mogelijk niet de nieuwste versie, echter automatisch bijwerken inschakelen zorgt u ervoor dat de meest recente update wordt altijd kunnen genieten van de Linux-Agent. Hebt u problemen bij het installeren van het Pakketbeheer, moet u zoeken naar ondersteuning van de leverancier van de distributie.

## <a name="minimum-virtual-machine-agent-support-in-azure"></a>Ondersteuning van de agent minimumgrootte van de virtuele machine in Azure
Controleer of de [minimaal versie-ondersteuning voor agents van de virtuele machine in Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) voordat u doorgaat.

## <a name="updating-the-azure-linux-agent"></a>De Azure Linux Agent bijwerken

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Pakketcache bijwerken

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

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

### <a name="debian-7-wheezy"></a>Debian 7 "Wheezy"

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Pakketcache bijwerken

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Agent automatisch bijwerken inschakelen
Deze versie van Debian beschikt niet over een versie > = 2.0.16, automatisch bijwerken is daarom niet beschikbaar. De uitvoer van de bovenstaande opdracht wordt weergegeven als het pakket bijgewerkt is.

### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 "Jessie" / Debian 9 'uitrekken"

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
apt list --installed | grep waagent
```

#### <a name="update-package-cache"></a>Pakketcache bijwerken

```bash
sudo apt-get -qq update
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```
sudo systemctl restart walinuxagent.service
```

## <a name="red-hat--centos"></a>Red Hat / CentOS

### <a name="rhelcentos-6"></a>RHEL/CentOS 6

#### <a name="check-your-current-package-version"></a>Controleer uw huidige Pakketversie

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Controleren op beschikbare updates

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

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

#### <a name="check-available-updates"></a>Controleren op beschikbare updates

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

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

#### <a name="check-available-updates"></a>Controleren op beschikbare updates

De bovenstaande uitvoer wordt weergegeven als het pakket bijgewerkt is.

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

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

#### <a name="check-available-updates"></a>Controleren op beschikbare updates

In de uitvoer van de bovenstaande, wordt dit beschreven als het pakket bijgewerkt is.

#### <a name="install-the-latest-package-version"></a>Installeer de nieuwste Pakketversie

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld 

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-the-waagent-service"></a>De waagent-service opnieuw starten

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a>Oracle 6 en 7

Voor Oracle Linux, zorg ervoor dat de `Addons` opslagplaats is ingeschakeld. Kies het bestand bewerken `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) of `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), en wijzigt u de regel `enabled=0` naar `enabled=1` onder **[ol6_addons]** of **[ol7_addons]** in deze het bestand.

Typ vervolgens de volgende opdracht voor het installeren van de meest recente versie van de Azure Linux Agent:

```bash
sudo yum install WALinuxAgent
```

Als u niet de opslagplaats van de invoegtoepassing kunt u deze regels gewoon toevoegen aan het einde van het bestand .repo op basis van uw Oracle Linux-versie:

Voor virtuele machines van Oracle Linux 6:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Voor virtuele machines van Oracle Linux 7:

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

Meestal dit is alles wat u nodig hebt, maar als om een bepaalde reden u moet voor het installeren van https://github.com rechtstreeks, gebruik de volgende stappen.


## <a name="update-the-linux-agent-when-no-agent-package-exists-for-distribution"></a>De Linux-Agent bijwerken wanneer er geen agentpakket voor distributie

Wget (Er zijn bepaalde distributies die niet standaard, zoals de versies van Red Hat, CentOS en Oracle Linux 6.4 en 6.5 installeert) installeren door te typen `sudo yum install wget` op de opdrachtregel.

### <a name="1-download-the-latest-version"></a>1. Download de nieuwste versie
Open [de release van Azure Linux Agent in GitHub](https://github.com/Azure/WALinuxAgent/releases) in een webpagina en ontdek het nummer van de meest recente versie. (U kunt uw huidige versie vinden door te typen `waagent --version`.)

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

### <a name="2-install-the-azure-linux-agent"></a>2. De Azure Linux-Agent installeren

#### <a name="for-version-22x-use"></a>Voor versie 2.2.x, gebruik:
Mogelijk moet u het pakket installeren `setuptools` ten eerste-- [hier](https://pypi.python.org/pypi/setuptools). Voer vervolgens

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Zorg ervoor dat automatisch bijwerken is ingeschakeld

Controleer eerst of deze is ingeschakeld:

```bash
cat /etc/waagent.conf
```

Zoek 'AutoUpdate.Enabled'. Als u deze uitvoer ziet, is het ingeschakeld:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Om in te schakelen uitvoeren:

```bash
sudo sed -i 's/# AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-the-waagent-service"></a>3. De waagent-service opnieuw starten
Voor het merendeel van de Linux-distributies:

```bash
sudo service waagent restart
```

Gebruik voor Ubuntu:

```bash
sudo service walinuxagent restart
```

CoreOS, gebruiken:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-the-azure-linux-agent-version"></a>4. Controleren welke versie van Azure Linux Agent
    
```bash
waagent -version
```

Voor CoreOS, is het mogelijk dat de bovenstaande opdracht niet werkt.

U ziet dat de versie van Azure Linux Agent is bijgewerkt naar de nieuwe versie.

Zie voor meer informatie over de Azure Linux Agent [Azure Linux Agent Leesmij](https://github.com/Azure/WALinuxAgent).
