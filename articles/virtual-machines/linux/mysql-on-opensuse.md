---
title: MySQL installeren op een OpenSUSE-VM in Azure | Microsoft Docs
description: Leer hoe u MySQL installeren op een OpenSUSE Linux VMirtual virtuele machine in Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: cynthn
ms.openlocfilehash: 88bd895cb3a384f1ada0394fe2da206aca86b981
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38670927"
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>MySQL op een a virtuele machine met OpenSUSE Linux installeren in Azure

[MySQL](http://www.mysql.com) is een populaire, open source-SQL-database. Deze zelfstudie leert u hoe u een virtuele machine met OpenSUSE Linux maken en vervolgens MySQL installeren.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u Azure CLI versie 2.0 of hoger nodig. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Een virtuele machine met OpenSUSE Linux maken

Maak eerst een resourcegroep. In dit voorbeeld zijn we naamgeving van de resourcegroep *mySQSUSEResourceGroup* en te maken in de *VS-Oost* regio.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

De virtuele machine maken. In dit voorbeeld zijn we naamgeving van de virtuele machine *myVM*. We gaan ook gebruik van een VM-grootte *Standard_D2s_v3*, maar u moet de [VM-grootte](sizes.md) u denkt dat het meest geschikt is voor uw workload.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

U moet ook een regel toevoegen aan de netwerkbeveiligingsgroep verkeer via poort 3306 is toegestaan voor MySQL.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

U gebruikt SSH verbinding maken met de virtuele machine. In dit voorbeeld wordt het openbare IP-adres van de virtuele machine is *10.111.112.113*. Wanneer u de virtuele machine hebt gemaakt, kunt u het IP-adres in de uitvoer zien.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## <a name="update-the-vm"></a>De virtuele machine bijwerken
 
Nadat u met de virtuele machine verbonden bent, installeert systeemupdates en patches. 
   
```bash
sudo zypper update
```

Volg de aanwijzingen voor het bijwerken van uw virtuele machine.

## <a name="install-mysql"></a>MySQL installeren 


De MySQL installeren op de virtuele machine via SSH. Reageren op prompts waar nodig.

```bash
sudo zypper install mysql
```
 
Stel MySQL te starten wanneer het systeem wordt opgestart. 

```bash
sudo systemctl enable mysql
```
Controleer of MySQL is ingeschakeld.

```bash
systemctl is-enabled mysql
```

Dit moet worden geretourneerd: ingeschakeld.


## <a name="mysql-password"></a>MySQL-wachtwoord

Na de installatie is de MySQL-hoofdwachtwoord standaard leeg. Voer de **mysql\_beveiligde\_installatie** script voor het beveiligen van MySQL. Het script vraagt u om te wijzigen van de MySQL-hoofdwachtwoord, anonieme gebruiker-accounts verwijderen, uitschakelen van externe hoofdmap aanmeldingen, test-databases verwijderen en opnieuw laden van de tabel bevoegdheden. 


```bash
mysql_secure_installation
```

## <a name="log-in-to-mysql"></a>Meld u aan bij MySQL

Nu kunt u zich aanmeldt en voer de MySQL-prompt.

```bash  
mysql -u root -p
```
Hiermee schakelt u naar de prompt MySQL waar u de SQL-instructies gebruikt om te communiceren met de database kan geven.

Maak nu een nieuwe MySQL-gebruiker.

```   
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
De puntkomma (;) aan het einde van de regel is van cruciaal belang voor het beëindigen van de opdracht.


## <a name="create-a-database"></a>Een database maken


Een database maken en verlenen de `mysqluser` gebruikersmachtigingen.

```   
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Database-gebruikersnamen en wachtwoorden worden alleen gebruikt door verbinding te maken met de database-scripts.  De namen van gebruikersaccounts database noodzakelijkerwijs niet werkelijke gebruikersaccounts op het systeem.

Schakel aanmelden vanaf een andere computer. In dit voorbeeld wordt het IP-adres van de computer die we aanmelden willen vanaf is *10.112.113.114*.

```   
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
Om af te sluiten van het hulpprogramma MySQL-database-beheer, typt u:

```    
quit
```


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over MySQL de [MySQL documentatie](http://dev.mysql.com/doc/index-topic.html).




