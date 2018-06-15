---
title: 'Zelfstudie: LEMP implementeren op een virtuele Linux-machine in Azure | Microsoft Docs'
description: In deze zelfstudie leert u hoe de LEMP-stack installeert op een virtuele Linux-machine in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: danlep
ms.openlocfilehash: 1f4c74ab96fc9494d1b3c7dd626265516533330b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32188460"
---
# <a name="tutorial-install-a-lemp-web-server-on-a-linux-virtual-machine-in-azure"></a>Zelfstudie: Een LEMP-webserver installeren op een virtuele Linux-machine in Azure

Dit artikel begeleidt u bij de implementatie van een NGINX-webserver, MySQL en PHP (de LEMP-stack) op een Ubuntu-VM in Azure. De LEMP-stack is een alternatief voor de populaire [LAMP-stack](tutorial-lamp-stack.md), die u ook in Azure kunt installeren. Als u de LEMP-server in actie wilt zien, kunt u eventueel een WordPress-site installeren en configureren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Ubuntu-VM maken (de L in LEMP-stack)
> * Poort 80 openen voor webverkeer
> * NGINX, MySQL en PHP installeren
> * Installatie en configuratie verifiëren
> * WordPress op de LEMP-server installeren

Deze installatie is voor snelle tests en het testen van het concept.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.30 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>NGINX, MySQL en PHP installeren

Voer de volgende opdracht uit om Ubuntu-pakketbronnen bij te werken en NGINX, MySQL en PHP te installeren. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

U wordt gevraagd de pakketten en andere afhankelijkheden te installeren. Stel, als daarom wordt gevraagd, een hoofdwachtwoord in voor MySQL en druk op Enter om door te gaan. Volg de resterende aanwijzingen op. In dit proces worden de minimaal vereiste PHP-extensies geïnstalleerd die voor het gebruik van PHP met MySQL nodig zijn. 

![Pagina met MySQL-hoofdwachtwoord][1]

## <a name="verify-installation-and-configuration"></a>Installatie en configuratie verifiëren


### <a name="nginx"></a>NGINX

Controleer de versie van NGINX met de volgende opdracht:
```bash
nginx -v
```

Als NGINX is geïnstalleerd en poort 80 is geopend voor de VM, is de webserver nu toegankelijk vanaf internet. Als u de welkomstpagina van NGINX wilt weergeven, opent u een webbrowser en voert u het openbare IP-adres van de VM in. Gebruik het openbare IP-adres dat u hebt gebruikt om een SSH-verbinding met de VM te maken:

![Standaardpagina van NGINX][3]


### <a name="mysql"></a>MySQL

Controleer de versie van MySQL met de volgende opdracht (let op de hoofdletter `V` van de parameter):

```bash
mysql -V
```

Voer het script `mysql_secure_installation` uit om de installatie van MySQL te beveiligen. Als u slechts een tijdelijke server instelt, kunt u deze stap overslaan. 

```bash
mysql_secure_installation
```

Voer een hoofdwachtwoord voor MySQL in en configureer de beveiligingsinstellingen voor uw omgeving.

Als u MySQL-functies wilt uitproberen (MySQL-database maken, gebruikers toevoegen of configuratie-instellingen wijzigen), meldt u zich aan bij MySQL. Deze stap is niet noodzakelijk voor het voltooien van deze zelfstudie. 


```bash
mysql -u root -p
```

Als u klaar bent, sluit u de mysql-prompt door `\q` te typen.

### <a name="php"></a>PHP

Controleer de versie van PHP met de volgende opdracht:

```bash
php -v
```

Configureer NGINX om PHP FastCGI Process Manager (PHP-FPM) te kunnen gebruiken. Voer de volgende opdrachten uit om een back-up te maken van het oorspronkelijke configuratiebestand van het NGINX-serverblok en bewerk het oorspronkelijke bestand in een editor naar keuze:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

Vervang in de editor de inhoud van `/etc/nginx/sites-available/default` door het volgende. Zie de opmerkingen voor een verklaring van de instellingen. Vervang *yourPublicIPAddress* door het openbare IP-adres van uw VM en laat de resterende instellingen ongemoeid. Sla het bestand op.

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

Controleer de configuratie van NGINX op syntaxisfouten:

```bash
sudo nginx -t
```

Als de syntaxis juist is, start u NGINX opnieuw met de volgende opdracht:

```bash
sudo service nginx restart
```

Als u meer tests wilt uitvoeren, maakt u snel een PHP-infopagina om in een browser weer te geven. Met de volgende opdracht wordt de PHP-infopagina gemaakt:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Nu kunt u de zojuist gemaakte PHP-infopagina controleren. Open een browser en ga naar `http://yourPublicIPAddress/info.php`. Vervang het openbare IP-adres van uw VM. Het moet er ongeveer uitzien als in deze afbeelding.

![PHP-infopagina][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een LEMP-server in Azure geïmplementeerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Maken van een Ubuntu-VM
> * Poort 80 openen voor webverkeer
> * NGINX, MySQL en PHP installeren
> * Installatie en configuratie verifiëren
> * WordPress op de LEMP-stack installeren

Ga door naar de volgende zelfstudie om te leren hoe u webservers kunt beveiligen met behulp van SSL-certificaten.

> [!div class="nextstepaction"]
> [Webserver beveiligen met SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
