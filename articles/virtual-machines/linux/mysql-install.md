---
title: Instellen van MySQL op een Linux-VM in Azure | Microsoft Docs
description: Informatie over het installeren van de MySQL-stack op een Linux-machine (Ubuntu of Red Hat-OS) in Azure
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: cynthn
ms.openlocfilehash: 21ad3f9baf4b8e117f881d9a36fc606af04e17a5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66158445"
---
# <a name="how-to-install-mysql-on-azure"></a>MySQL installeren op Azure
In dit artikel leert u hoe u kunt installeren en configureren van MySQL op een Azure-machine waarop Linux wordt uitgevoerd.


> [!NOTE]
> U moet al een Microsoft Azure-virtuele machine waarop Linux wordt uitgevoerd om te kunnen voltooien van deze zelfstudie hebben. Raadpleeg de [zelfstudie voor Azure Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) maken en instellen van een Linux-VM met `mysqlnode` als naam van de VM en `azureuser` als gebruiker voordat u doorgaat.
> 
> 

In dit geval poort 3306 gebruiken als de MySQL-poort.  

We gebruiken opslagplaats pakket MySQL5.6 installeren als een voorbeeld in dit artikel. MySQL5.6 heeft eigenlijk meer verbetering in de prestaties dan MySQL5.5.  Meer informatie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

## <a name="install-mysql56-on-ubuntu"></a>MySQL5.6 installeren op Ubuntu
We gebruiken een Linux-VM met Ubuntu.


### <a name="install-mysql"></a>MySQL installeren

5\.6 voor MySQL-Server installeren door het overschakelen naar de `root` gebruiker:

```bash  
sudo su -
```

Mysql-server 5.6 installeren:

```bash  
apt-get update
apt-get -y install mysql-server-5.6
```

  
Tijdens de installatie, ziet u een dialoogvenster weergegeven om aan te vragen u om in te stellen van de MySQL-hoofdwachtwoord hieronder en moet u hier het wachtwoord instelt.
  
![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

Voer het wachtwoord nogmaals in ter bevestiging.

![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

### <a name="sign-in"></a>Aanmelden
  
Wanneer de installatie van MySQL-server is voltooid, MySQL-service automatisch gestart. U kunt zich aanmelden met MySQL-Server met de `root` gebruiker, en voer uw wachtwoord in.

```bash  
mysql -uroot -p
```


### <a name="manage-the-mysql-service"></a>De MySQL-service beheren

Status van de MySQL-service ophalen

```bash   
service mysql status
```
  
MySQL-Service starten

```bash  
service mysql start
```
  
MySQL-service stoppen

```bash  
service mysql stop
```
  
De MySQL-service opnieuw starten

```bash  
service mysql restart
```

## <a name="install-mysql-on-red-hat-os-centos-oracle-linux"></a>MySQL installeren op OS van Red Hat, CentOS, Oracle Linux
We gebruiken Linux-VM hier bij CentOS of Oracle Linux.

### <a name="add-the-mysql-yum-repository"></a>De MySQL-yum-opslagplaats toevoegen
    
Schakel over naar `root` gebruiker:

```bash  
sudo su -
```

Download en installeer het releasepakket van MySQL:

```bash  
wget https://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
yum localinstall -y mysql-community-release-el6-5.noarch.rpm
```

### <a name="enable-the-mysql-repository"></a>De MySQL-opslag inschakelen
Onder bestand om te schakelen van de MySQL-opslagplaats voor het downloaden van het pakket MySQL5.6 bewerken.

```bash  
vim /etc/yum.repos.d/mysql-community.repo
```

  
Werk elke waarde van dit bestand hieronder:

```  
\# *Enable to use MySQL 5.6*
  
[mysql56-community]
name=MySQL 5.6 Community Server
  
baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
enabled=1
  
gpgcheck=1
  
gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### <a name="install-mysql"></a>MySQL installeren 

MySQL installeren vanuit de opslagplaats.

```bash  
yum install mysql-community-server
```
  
De MySQL-RPM-pakket en alle gerelateerde pakketten worden geïnstalleerd.


## <a name="manage-the-mysql-service"></a>De MySQL-service beheren
  
Controleer de status van de service van de MySQL-server:

```bash  
service mysqld status\
```
  
Controleer of de standaard poort van de MySQL-server wordt uitgevoerd:

```bash  
netstat  –tunlp|grep 3306
```

Start de MySQL-server:

```bash
service mysqld start
```

Stop de MySQL-server:

```bash
service mysqld stop
```

Instellen van MySQL te starten wanneer het systeem start-up:

```bash
chkconfig mysqld on
```

## <a name="install-mysql-on-suse-linux"></a>MySQL in SUSE Linux installeren

Er wordt hier Linux-VM met OpenSUSE gebruiken.

### <a name="download-and-install-mysql-server"></a>Download en installeer de MySQL-Server
  
Schakel over naar `root` gebruiker via de onderstaande opdracht:  

```bash  
sudo su -
```
  
Download en installeer de MySQL-pakket:

```bash  
zypper update
zypper install mysql-server mysql-devel mysql
```

### <a name="manage-the-mysql-service"></a>De MySQL-service beheren
  
Controleer de status van de MySQL-server:

```bash  
rcmysql status
```
  
Controleer of de standaard-poort van de MySQL-server:

```bash  
netstat  –tunlp|grep 3306
```

Start de MySQL-server:

```bash
rcmysql start
```

Stop de MySQL-server:

```bash
rcmysql stop
```

Instellen van MySQL te starten wanneer het systeem start-up:

```bash
insserv mysql
```

## <a name="next-step"></a>Volgende stap
Zie voor meer informatie de [MySQL](https://www.mysql.com/) website.

