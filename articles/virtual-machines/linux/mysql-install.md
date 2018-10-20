---
title: Instellen van MySQL op een Linux-VM in Azure | Microsoft Docs
description: Informatie over het installeren van de MySQL-stack op een Linux-machine (Ubuntu of Red Hat-OS) in Azure
services: virtual-machines-linux
documentationcenter: ''
author: zr-msft
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
ms.author: zarhoads
ms.openlocfilehash: f7120decd4a5d43f88b55e7d7e20992af34cadc4
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49469566"
---
# <a name="how-to-install-mysql-on-azure"></a>MySQL installeren op Azure
In dit artikel leert u hoe u kunt installeren en configureren van MySQL op een Azure-machine waarop Linux wordt uitgevoerd.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a>MySQL installeren op uw virtuele machine
> [!NOTE]
> U moet al een Microsoft Azure-virtuele machine waarop Linux wordt uitgevoerd om te kunnen voltooien van deze zelfstudie hebben. Raadpleeg de [zelfstudie voor Azure Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) maken en instellen van een Linux-VM met `mysqlnode` als naam van de VM en `azureuser` als gebruiker voordat u doorgaat.
> 
> 

In dit geval poort 3306 gebruiken als de MySQL-poort.  

Verbinding maken met de Linux VM die u hebt gemaakt via putty. Als dit de eerste keer dat u virtuele Azure Linux-machine gebruikt, ziet u hoe u putty verbinding maken met een Linux-VM [hier](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

We gebruiken opslagplaats pakket MySQL5.6 installeren als een voorbeeld in dit artikel. MySQL5.6 heeft eigenlijk meer verbetering in de prestaties dan MySQL5.5.  Meer informatie [hier](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).

### <a name="how-to-install-mysql56-on-ubuntu"></a>Over het installeren van MySQL5.6 op Ubuntu
Er wordt hier Linux-VM met Ubuntu van Azure gebruiken.

* Stap 1: Installatie van MySQL 5.6-Server overschakelen naar `root` gebruiker:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Mysql-server 5.6 installeren:
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    Tijdens de installatie ziet u een dialoogvenster venster poping tot vraagt u om in te stellen van de MySQL-hoofdwachtwoord hieronder, en u moet de hier wachtwoord instelt.
  
    ![image](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    Voer het wachtwoord nogmaals in ter bevestiging.

    ![image](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* Stap 2: Aanmelding MySQL-Server
  
    Wanneer de installatie van MySQL-server is voltooid, MySQL-service automatisch gestart. U kunt zich aanmelden met de MySQL-Server `root` gebruiker.
    Gebruik de onderstaande opdracht aanmeldings- en invoer-wachtwoord.
  
             #[root@mysqlnode ~]# mysql -uroot -p
* Stap 3: De service MySQL beheren
  
    (a) de status van de MySQL-service ophalen
  
             #[root@mysqlnode ~]# service mysql status
  
    (b) Start MySQL Service
  
             #[root@mysqlnode ~]# service mysql start
  
    (c) MySQL-service stoppen
  
             #[root@mysqlnode ~]# service mysql stop
  
    (d) de MySQL-service opnieuw starten
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>MySQL installeren op Red Hat OS-familie, zoals CentOS, Oracle Linux
We gebruiken Linux-VM hier bij CentOS of Oracle Linux.

* Stap 1: De MySQL Yum-opslagplaats Switch toevoegen aan `root` gebruiker:
  
            #[azureuser@mysqlnode:~]sudo su -
  
    Download en installeer het releasepakket van MySQL:
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* Stap 2: Bewerk onder bestand om te schakelen van de MySQL-opslagplaats voor het downloaden van het pakket MySQL5.6.
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    Werk elke waarde van dit bestand hieronder:
  
        \# *Enable to use MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* Stap 3: Installatie van MySQL van MySQL-opslagplaats MySQL installeren:
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    MySQL-RPM-pakket en alle gerelateerde pakketten worden geïnstalleerd.
* Stap 4: De service MySQL beheren
  
    (a) Controleer de status van de service van de MySQL-server:
  
           #[root@mysqlnode ~]#service mysqld status
  
    (b) Controleer of de standaard poort van de MySQL-server wordt uitgevoerd:
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    (c) start de MySQL-server:

           #[root@mysqlnode ~]#service mysqld start

    (d) de MySQL-server stoppen:

           #[root@mysqlnode ~]#service mysqld stop

    (e) set MySQL te starten wanneer het systeem start-up:

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-to-install-mysql-on-suse-linux"></a>MySQL in SUSE Linux installeren
Er wordt hier Linux-VM met OpenSUSE gebruiken.

* Stap 1: Download en installeer de MySQL-Server
  
    Schakel over naar `root` gebruiker via de onderstaande opdracht:  
  
           #sudo su -
  
    Download en installeer de MySQL-pakket:
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* Stap 2: De service MySQL beheren
  
    (a) Controleer de status van de MySQL-server:
  
           #[root@mysqlnode ~]# rcmysql status
  
    (b) Controleer of de standaard-poort van de MySQL-server:
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    (c) start de MySQL-server:

           #[root@mysqlnode ~]# rcmysql start

    (d) de MySQL-server stoppen:

           #[root@mysqlnode ~]# rcmysql stop

    (e) set MySQL te starten wanneer het systeem start-up:

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a>Volgende stap
Meer informatie en informatie met betrekking tot MySQL zoeken [hier](https://www.mysql.com/).

