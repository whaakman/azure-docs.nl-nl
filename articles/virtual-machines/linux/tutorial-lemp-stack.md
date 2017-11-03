---
title: LEMP implementeren op een virtuele Linux-machine in Azure | Microsoft Docs
description: Zelfstudie - installatie de stack LEMP op een Linux VM in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: 87d60ae51aaa33b709d272605419fd85eeb5d93d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Een webserver LEMP installeren op een virtuele machine in Azure
Dit artikel begeleidt u bij het implementeren van een NGINX-webserver, MySQL en PHP (de LEMP-stack) op een Ubuntu VM in Azure. De stack LEMP vormt een alternatief voor de populaire [licht stack](tutorial-lamp-stack.md), die u kunt ook installeren in Azure. Overzicht van de server LEMP in actie kunt u optioneel installeren en configureren van een WordPress-site. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een VM Ubuntu (de "L" in de stack LEMP)
> * Poort 80 openen voor webverkeer
> * NGINX, MySQL en PHP installeren
> * Controleer of de installatie en configuratie
> * WordPress installeren op de server LEMP


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>NGINX, MySQL en PHP installeren

Voer de volgende opdracht om te werken Ubuntu pakket bronnen en NGINX, MySQL en PHP installeren. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

U wordt gevraagd om de pakketten en andere afhankelijkheden te installeren. Wanneer u wordt gevraagd, een root-wachtwoord instellen voor MySQL, en vervolgens [Enter] om door te gaan. Volg de aanwijzingen resterende. Dit proces installeert de minimale vereiste PHP-uitbreidingen die nodig zijn voor PHP met MySQL gebruikt. 

![Pagina MySQL root-wachtwoord][1]

## <a name="verify-installation-and-configuration"></a>Controleer of de installatie en configuratie


### <a name="nginx"></a>NGINX

Controleer de versie van NGINX met de volgende opdracht:
```bash
nginx -v
```

Met NGINX geïnstalleerd, en poort 80 is geopend voor uw virtuele machine, zijn de webserver nu toegankelijk vanaf internet. Open een webbrowser om weer te geven de NGINX-welkomstpagina, en geef het openbare IP-adres van de virtuele machine. Gebruik het openbare IP-adres dat u hebt gebruikt voor SSH naar de virtuele machine:

![Standaard-NGINX-pagina][3]


### <a name="mysql"></a>MySQL

Controleer de versie van MySQL met de volgende opdracht (Let op het kapitaal `V` parameter):

```bash
mysql -V
```

U wordt aangeraden het volgende script voor het beveiligen van de installatie van MySQL uitgevoerd:

```bash
mysql_secure_installation
```

Voer uw MySQL root-wachtwoord en de beveiligingsinstellingen voor uw omgeving.

Als u maken van een MySQL-database wilt, gebruikers toevoegen of wijzigen van configuratie-instellingen, meld u aan bij de MySQL:

```bash
mysql -u root -p
```

Wanneer u klaar bent, sluit u de mysql-prompt door typen `\q`.

### <a name="php"></a>PHP

Controleer de versie van PHP met de volgende opdracht:

```bash
php -v
```

Configureer NGINX als PHP-FastCGI-proces Manager (PHP-Rambus) wilt gebruiken. Voer de volgende opdrachten voor back-up van het oorspronkelijke configuratiebestand blok van NGINX en bewerk het oorspronkelijke bestand in een editor naar keuze:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

Vervang de inhoud van in de editor `/etc/nginx/sites-available/default` met de volgende stappen. Zie de opmerkingen voor uitleg van de instellingen. Vervang het openbare IP-adres van uw virtuele machine voor *yourPublicIPAddress*, en laat de overige instellingen. Sla het bestand op.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Controleer de configuratie NGINX syntaxisfouten:

```bash
sudo nginx -t
```

Als de syntaxis juist is, start u NGINX opnieuw met de volgende opdracht:

```bash
sudo service nginx restart
```

Als u wilt om verder te testen, maakt u een snelle PHP pagina om weer te geven in een browser. De volgende opdracht maakt het PHP-info-pagina:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



U kunt nu controleren de PHP-info pagina die u hebt gemaakt. Open een browser en Ga naar `http://yourPublicIPAddress/info.php`. Vervang het openbare IP-adres van uw virtuele machine. Het moet eruitzien als aan deze installatiekopie.

![Pagina voor PHP-info][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een server LEMP in Azure geïmplementeerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een virtuele Ubuntu-machine maken
> * Poort 80 openen voor webverkeer
> * NGINX, MySQL en PHP installeren
> * Controleer of de installatie en configuratie
> * WordPress installeren op de stack LEMP

Ga naar de volgende zelfstudie voor meer informatie over het beveiligen van webservers met SSL-certificaten.

> [!div class="nextstepaction"]
> [Webserver met SSL beveiligde](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
