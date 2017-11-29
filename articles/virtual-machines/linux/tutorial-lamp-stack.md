---
title: LICHT implementeren op een virtuele Linux-machine in Azure | Microsoft Docs
description: Zelfstudie - installatie de stack licht op een Linux VM in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: danlep
ms.openlocfilehash: 8fcf411db844e227e0c4db0e690a1832f98b42f1
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Een webserver licht installeren op een virtuele machine in Azure
Dit artikel begeleidt u bij het implementeren van een Apache-webserver, MySQL en PHP (de licht-stack) op een Ubuntu VM in Azure. Als u liever de NGINX-webserver, Zie de [LEMP stack](tutorial-lemp-stack.md) zelfstudie. Overzicht van de server licht in actie kunt u optioneel installeren en configureren van een WordPress-site. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een VM Ubuntu (de "L" in de stack licht)
> * Poort 80 openen voor webverkeer
> * Apache, MySQL en PHP installeren
> * Controleer of de installatie en configuratie
> * WordPress installeren op de server licht


Deze instelling is voor de snelle tests noch een bewijs van het concept. Zie voor meer informatie over de stack licht, aanbevelingen voor een productie-omgeving, inclusief de [Ubuntu documentatie](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Apache, MySQL en PHP installeren

Voer de volgende opdracht om te werken Ubuntu pakket bronnen en Apache, MySQL en PHP installeren. Houd er rekening mee de dakje (^) aan het einde van de opdracht die deel uitmaakt van de `lamp-server^` pakketnaam. 


```bash
sudo apt update && sudo apt install lamp-server^
```


U wordt gevraagd om de pakketten en andere afhankelijkheden te installeren. Wanneer u wordt gevraagd, een root-wachtwoord instellen voor MySQL, en vervolgens [Enter] om door te gaan. Volg de aanwijzingen resterende. Dit proces installeert de minimale vereiste PHP-uitbreidingen die nodig zijn voor PHP met MySQL gebruikt. 

![Pagina MySQL root-wachtwoord][1]

## <a name="verify-installation-and-configuration"></a>Controleer of de installatie en configuratie


### <a name="apache"></a>Apache

Controleer de versie van Apache met de volgende opdracht:
```bash
apache2 -v
```

Met Apache geïnstalleerd en poort 80 is geopend voor uw virtuele machine, zijn de webserver nu toegankelijk vanaf internet. Als de Apache2 Ubuntu standaardpagina, open een webbrowser en geef het openbare IP-adres van de virtuele machine. Gebruik het openbare IP-adres dat u hebt gebruikt voor SSH naar de virtuele machine:

![Apache standaardpagina][3]


### <a name="mysql"></a>MySQL

Controleer de versie van MySQL met de volgende opdracht (Let op het kapitaal `V` parameter):

```bash
mysql -V
```

Ter beveiliging van de installatie van MySQL uitvoeren de `mysql_secure_installation` script. Als u alleen een tijdelijke server instelt, kunt u deze stap overslaan.

```bash
mysql_secure_installation
```

Geef een root-wachtwoord voor MySQL en de beveiligingsinstellingen voor uw omgeving.

Als u wilt proberen MySQL-functies (een MySQL-database maken, gebruikers toevoegen of wijzigen van configuratie-instellingen), meld u aan bij MySQL. Deze stap is niet vereist om deze zelfstudie te voltooien.

```bash
mysql -u root -p
```

Wanneer u klaar bent, sluit u de mysql-prompt door typen `\q`.

### <a name="php"></a>PHP

Controleer de versie van PHP met de volgende opdracht:

```bash
php -v
```

Als u wilt om verder te testen, maakt u een snelle PHP pagina om weer te geven in een browser. De volgende opdracht maakt het PHP-info-pagina:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

U kunt nu controleren de PHP-info pagina die u hebt gemaakt. Open een browser en Ga naar `http://yourPublicIPAddress/info.php`. Vervang het openbare IP-adres van uw virtuele machine. Het moet eruitzien als aan deze installatiekopie.

![Pagina voor PHP-info][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een server licht in Azure geïmplementeerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtuele Ubuntu-machine maken
> * Poort 80 openen voor webverkeer
> * Apache, MySQL en PHP installeren
> * Controleer of de installatie en configuratie
> * WordPress installeren op de server licht

Ga naar de volgende zelfstudie voor meer informatie over het beveiligen van webservers met SSL-certificaten.

> [!div class="nextstepaction"]
> [Webserver met SSL beveiligde](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png