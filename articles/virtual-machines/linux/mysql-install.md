---
title: Instellen van MySQL op een Linux VM in Azure | Microsoft Docs
description: Informatie over het installeren van de MySQL-stack op een virtuele Linux-machine (Ubuntu of RedHat familie OS) in Azure
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: 0ee70bda954cf0a193d43b5b47702e7b2c37844d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-install-mysql-on-azure"></a>MySQL installeren op Azure
In dit artikel leert u hoe installeren en configureren van MySQL in Azure een virtuele machine waarop Linux wordt uitgevoerd.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>MySQL installeren op de virtuele machine
> [!NOTE]
> U moet al een Microsoft Azure virtuele machine waarop Linux wordt uitgevoerd om te kunnen voltooien van deze zelfstudie hebben. Zie de [Azure Linux VM-zelfstudie](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) maken en instellen van een Linux-VM met `mysqlnode` als naam van de VM en `azureuser` als gebruiker voordat u doorgaat.
> 
> 

In dit geval 3306 poort gebruiken als de MySQL-poort.  

Verbinding maken met de Linux VM die u hebt gemaakt via de putty. Als dit de eerste keer dat u Azure Linux VM gebruikt, ziet u hoe u met putty verbinding maken met een Linux-VM [hier](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

We gebruiken opslagplaats pakket MySQL5.6 installeren als een voorbeeld in dit artikel. MySQL5.6 heeft eigenlijk meer verbetering in prestaties dan MySQL5.5.  Meer informatie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-to-install-mysql56-on-ubuntu"></a>MySQL5.6 op Ubuntu installeren
We gebruiken Linux VM hier met Ubuntu van Azure.

* Stap 1: Installeer MySQL Server 5.6 overschakelen naar `root` gebruiker:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Mysql-server 5.6 installeren:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Tijdens de installatie ziet u een dialoogvenster venster poping tot vragen u hieronder MySQL root-wachtwoord instellen, en u moet de hier wachtwoord instelt.
  
    ![Afbeelding](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Voer het wachtwoord ter bevestiging nogmaals op.

    ![Afbeelding](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* Stap 2: Aanmelding MySQL-Server
  
    Wanneer de installatie van de MySQL-server is voltooid, wordt MySQL-service automatisch gestart. U kunt zich aanmelden MySQL-Server met `root` gebruiker.
    Gebruik de onderstaande opdracht voor het wachtwoord voor aanmelding en invoer.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* Stap 3: De MySQL service beheren
  
    (a) de status van MySQL-service ophalen
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) MySQL-Service starten
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) MySQL-service stoppen
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) de MySQL-service opnieuw starten
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Het installeren van MySQL op Red Hat OS-familie zoals CentOS, Oracle Linux
We gebruiken Linux VM hier met de CentOS, Oracle Linux.

* Stap 1: De opslagplaats MySQL Yum Switch toevoegen aan `root` gebruiker:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Download en installeer het releasepakket van MySQL:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* Stap 2: Bewerken hieronder bestand zodat de MySQL-opslagplaats voor het downloaden van het pakket MySQL5.6.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Update voor elke waarde van dit bestand hieronder:
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* Stap 3: Installatie MySQL van MySQL-opslagplaats MySQL installeren:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    MySQL-RPM-pakket en alle gerelateerde pakketten worden geïnstalleerd.
* Stap 4: De MySQL service beheren
  
    (a) Controleer de servicestatus van de MySQL-server:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) Controleer of de standaard poort van MySQL-server actief is:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) de MySQL-server niet starten:

           #[root@mysqlnode ~]#service mysqld start

    (d) stoppen van de MySQL-server:

           #[root@mysqlnode ~]#service mysqld stop

    (e) MySQL ingesteld om te starten wanneer het systeem boot-up:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a>MySQL op SUSE Linux installeren
We gebruiken Linux VM hier met OpenSUSE.

* Stap 1: Download en installeer MySQL-Server
  
    Overschakelen naar `root` gebruiker via de onderstaande opdracht:  
  
           #sudo su -
  
    Download en installeer MySQL pakket:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* Stap 2: De MySQL service beheren
  
    (a) Controleer de status van de MySQL-server:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) Controleer of de standaardpoort van de MySQL-server:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) de MySQL-server niet starten:

           #[root@mysqlnode ~]# rcmysql start

    (d) stoppen van de MySQL-server:

           #[root@mysqlnode ~]# rcmysql stop

    (e) MySQL ingesteld om te starten wanneer het systeem boot-up:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Volgende stap
Meer informatie over het gebruik en informatie met betrekking tot MySQL vinden [hier](https://www.mysql.com/).

