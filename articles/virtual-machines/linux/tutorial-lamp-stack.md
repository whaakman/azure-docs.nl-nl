---
title: 'Zelfstudie: LAMP implementeren op een virtuele Linux-machine in Azure | Microsoft Docs'
description: In deze zelfstudie leert u hoe de LAMP-stack installeert op een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: cynthn
ms.openlocfilehash: 66b7d7692d9143c8db813ad135b0b9c70b8869d2
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708591"
---
# <a name="tutorial-install-a-lamp-web-server-on-a-linux-virtual-machine-in-azure"></a>Zelfstudie: Een LAMP-webserver installeren op een virtuele Linux-machine in Azure

Dit artikel begeleidt u bij de implementatie van een Apache-webserver, MySQL en PHP (de LAMP-stack) op een Ubuntu-VM in Azure. Zie [LEMP-stack](tutorial-lemp-stack.md) als u de voorkeur geeft aan de NGINX-webserver. Als u de LAMP-server in actie wilt zien, kunt u eventueel een WordPress-site installeren en configureren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Ubuntu-VM maken (de L in LAMP-stack)
> * Poort 80 openen voor webverkeer
> * Apache, MySQL en PHP installeren
> * Installatie en configuratie verifiëren
> * WordPress op de LAMP-server installeren

Deze installatie is voor snelle tests en het testen van het concept. Zie de [documentatie van Ubuntu](https://help.ubuntu.com/community/ApacheMySQLPHP) (Engelstalig) voor meer informatie over de LAMP-stack, waaronder aanbevelingen voor een productieomgeving.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.30 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Apache, MySQL en PHP installeren

Voer de volgende opdracht uit om Ubuntu-pakketbronnen bij te werken en Apache, MySQL en PHP te installeren. Let op het caret-teken (^) aan het eind van de opdracht. Dit maakt deel uit van de pakketnaam van `lamp-server^`. 


```bash
sudo apt update && sudo apt install lamp-server^
```

U wordt gevraagd de pakketten en andere afhankelijkheden te installeren. In dit proces worden de minimaal vereiste PHP-extensies geïnstalleerd die voor het gebruik van PHP met MySQL nodig zijn.  

## <a name="verify-installation-and-configuration"></a>Installatie en configuratie verifiëren


### <a name="verify-apache"></a>Apache Verifiëren

Controleer de versie van Apache met de volgende opdracht:
```bash
apache2 -v
```

Als Apache is geïnstalleerd en poort 80 is geopend voor de VM, is de webserver nu toegankelijk vanaf internet. Als u de standaardpagina van Apache2 Ubuntu wilt weergeven, opent u een webbrowser en voert u het openbare IP-adres van de VM in. Gebruik het openbare IP-adres dat u hebt gebruikt om een SSH-verbinding met de VM te maken:

![Standaardpagina van Apache][3]


### <a name="verify-and-secure-mysql"></a>MySQL verifiëren en beveiligen

Controleer de versie van MySQL met de volgende opdracht (let op de hoofdletter `V` van de parameter):

```bash
mysql -V
```

Als u de installatie van MySQL wilt beveiligen, inclusief het instellen van een hoofdwachtwoord, voert u het script `mysql_secure_installation` uit. 

```bash
sudo mysql_secure_installation
```

U kunt desgewenst de invoegtoepassing voor wachtwoordvalidatie instellen (aanbevolen). Vervolgens stelt u een wachtwoord in voor de MySQL-hoofdgebruiker en configureert u de resterende instellingen voor uw omgeving. We raden u aan alle vragen met 'J' (Ja) te beantwoorden.

Als u MySQL-functies wilt uitproberen (MySQL-database maken, gebruikers toevoegen of configuratie-instellingen wijzigen), meldt u zich aan bij MySQL. Deze stap is niet noodzakelijk voor het voltooien van deze zelfstudie.

```bash
sudo mysql -u root -p
```

Als u klaar bent, sluit u de mysql-prompt door `\q` te typen.

### <a name="verify-php"></a>PHP verifiëren

Controleer de versie van PHP met de volgende opdracht:

```bash
php -v
```

Als u meer tests wilt uitvoeren, maakt u snel een PHP-infopagina om in een browser weer te geven. Met de volgende opdracht wordt de PHP-infopagina gemaakt:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Nu kunt u de zojuist gemaakte PHP-infopagina controleren. Open een browser en ga naar `http://yourPublicIPAddress/info.php`. Vervang het openbare IP-adres van uw VM. Het moet er ongeveer uitzien als in deze afbeelding.

![PHP-infopagina][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een LAMP-server in Azure geïmplementeerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maken van een Ubuntu-VM
> * Poort 80 openen voor webverkeer
> * Apache, MySQL en PHP installeren
> * Installatie en configuratie verifiëren
> * WordPress op de LAMP-server installeren

Ga door naar de volgende zelfstudie om te leren hoe u webservers kunt beveiligen met behulp van SSL-certificaten.

> [!div class="nextstepaction"]
> [Webserver beveiligen met SSL](tutorial-secure-web-server.md)

[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png
