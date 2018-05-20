---
title: SSH-ondersteuning voor Azure App Service op Linux | Microsoft Docs
description: Meer informatie over het gebruik van SSH met Azure App Service op Linux.
keywords: Azure app service, web-app, linux, oss
services: app-service
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: c2beb67a27b667d31402b903f38dbf116e9425d0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>SSH-ondersteuning voor Azure App Service op Linux

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) wordt meestal gebruikt om op afstand beheerdersrechten opdrachten uitvoeren vanaf een opdrachtregel terminal. Op Linux-App Service biedt SSH-ondersteuning in de app-container met elk van de ingebouwde Docker-installatiekopieën gebruikt voor de Runtime-Stack van nieuwe web-apps. 

![Runtime-Stacks](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Voor aangepaste Docker-installatiekopieën, door het configureren van SSH-server in uw aangepaste installatiekopie.

U kunt ook verbinding maken met de container rechtstreeks vanuit uw lokale ontwikkelcomputer via SSH- en SFTP.

## <a name="open-ssh-session-in-browser"></a>SSH-sessie openen in browser

Als u een SSH-clientverbinding met de container, moet uw app worden uitgevoerd.

Plak de volgende URL in de browser en vervang \<app_naam > met de appnaam van uw:

```
https://<app_name>.scm.azurewebsites.net/webssh/host
```

Als u zijn niet geverifieerd, moet u met uw Azure-abonnement om verbinding te verifiëren. Eenmaal is geverifieerd, ziet u een shell in de browser, waarbij u opdrachten kunt uitvoeren in de container.

![SSH-verbinding](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="use-ssh-support-with-custom-docker-images"></a>Ondersteuning voor SSH gebruiken met aangepaste Docker-installatiekopieën

Om een aangepaste installatiekopie Docker ter ondersteuning van SSH-communicatie tussen de container en de client in de Azure portal, moet u de volgende stappen uitvoeren voor uw Docker-installatiekopie.

Deze stappen worden weergegeven in de Azure App Service-opslagplaats als [een voorbeeld](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

1. Bevatten de `openssh-server` installatie in [ `RUN` instructie](https://docs.docker.com/engine/reference/builder/#run) in de Dockerfile voor uw installatiekopie en het wachtwoord voor de basis-account toe aan set `"Docker!"`.

    > [!NOTE]
    > Deze configuratie staat geen externe verbindingen naar de container toe. SSH kan alleen worden benaderd via de Kudu / SCM-Site, die is geverifieerd met de referenties van de publicatie.

    ```docker
    # ------------------------
    # SSH Server support
    # ------------------------
    RUN apt-get update \
        && apt-get install -y --no-install-recommends openssh-server \
        && echo "root:Docker!" | chpasswd
    ```

1. Toevoegen een [ `COPY` instructie](https://docs.docker.com/engine/reference/builder/#copy) naar de Dockerfile kopiëren een [sshd_config](http://man.openbsd.org/sshd_config) van het bestand in de */enzovoort/ssh/* directory. Het configuratiebestand moet worden gebaseerd op het bestand sshd_config in de Azure App Service-GitHub-opslagplaats [hier](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

    > [!NOTE]
    > De *sshd_config* bestand moet bevatten de volgende of de verbinding is mislukt: 
    > * `Ciphers` moet ten minste een van de volgende opnemen: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs` moet ten minste een van de volgende opnemen: `hmac-sha1,hmac-sha1-96`.

    ```docker
    COPY sshd_config /etc/ssh/
    ```

1. Poort 2222 in omvatten de [ `EXPOSE` instructie](https://docs.docker.com/engine/reference/builder/#expose) voor de Dockerfile. Hoewel het hoofdwachtwoord bekend is, is poort 2222 niet toegankelijk vanaf het internet. Het is een interne alleen poort toegankelijk alleen door containers in het netwerk bridge van een virtueel particulier netwerk.

    ```docker
    EXPOSE 2222 80
    ```

1. Zorg ervoor dat de SSH-service met behulp van een shell-script te starten (Zie het voorbeeld op [init_container.sh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh)).

    ```bash
    #!/bin/bash
    service ssh start
    ```

Maakt gebruik van de Dockerfile de [ `ENTRYPOINT` instructie](https://docs.docker.com/engine/reference/builder/#entrypoint) het script uit te voeren.

    ```docker
    COPY startup /opt/startup
    ...
    RUN chmod 755 /opt/startup/init_container.sh
    ...
    ENTRYPOINT ["/opt/startup/init_container.sh"]
    ```

## <a name="open-ssh-session-from-remote-shell"></a>SSH-sessie openen vanuit externe shell

> [!NOTE]
> Deze functie is momenteel in Preview.
>

Met TCP tunneling u kunt een netwerkverbinding tussen uw ontwikkelcomputer en Web-App voor Containers via een geverifieerde WebSocket-verbinding maken. Hiermee kunt u een SSH-sessie openen met uw container uitgevoerd in App Service van de client van uw keuze.

Om te beginnen, moet u eerst installeren [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Als u wilt zien hoe het werkt zonder Azure CLI installeren, openen [Azure Cloud Shell](../../cloud-shell/overview.md). 

De meest recente App Service-extensie toevoegen door te voeren [az-extensie toevoegen](/cli/azure/extension?view=azure-cli-latest#az-extension-add):

```azurecli-interactive
az extension add -–name webapp
```

Als u al hebt uitgevoerd `az extension add` voordat, uitvoert [az extensie update](/cli/azure/extension?view=azure-cli-latest#az-extension-update) in plaats daarvan:

```azurecli-interactive
az extension update -–name webapp
```

Open een externe verbinding met uw app met de [az webapp-verbinding met extern maken](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) opdracht. Geef  _\<groep\_name >_ en \_< app\_name > _ voor uw app en vervang \<poort > met een lokale poortnummer.

```azurecli-interactive
az webapp remote-connection create --resource-group <group_name> -n <app_name> -p <port> &
```

> [!TIP]
> `&` aan het einde van de opdracht is alleen voor uw gemak bedoeld; als u van Cloud-Shell gebruikmaakt. Deze wordt het proces op de achtergrond uitgevoerd, zodat u de volgende opdracht in de dezelfde shell uitvoeren kunt.

Uitvoer van de opdracht geeft u de informatie die u moet een SSH-sessie openen.

```
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Open een SSH-sessie met de container met de client van uw keuze met behulp van de lokale poort. Het volgende voorbeeld wordt de standaardwaarde [ssh](https://ss64.com/bash/ssh.html) opdracht:

```azurecli-interactive
ssh root@127.0.0.1 -p <port>
```

Wanneer u wordt gevraagd, typt u `yes` om door te gaan verbinding te maken. U wordt gevraagd om het wachtwoord. Gebruik `Docker!`, die u eerder is aangegeven.

```
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Als u bent geverifieerd, ziet u het welkomstscherm van de sessie.

```
  _____
  /  _  \ __________ _________   ____
 /  /_\  \___   /  |  \_  __ \_/ __ \
/    |    \/    /|  |  /|  | \/\  ___/
\____|__  /_____ \____/ |__|    \___  >
        \/      \/                  \/
A P P   S E R V I C E   O N   L I N U X

0e690efa93e2:~#
```

U hebt nu verbinding met de connector. 

Probeer die wordt uitgevoerd de [boven](https://ss64.com/bash/top.html) opdracht. U moet mogelijk zijn om te zien hoe uw app werkt in de lijst. In het onderstaande voorbeeld is de knop met de `PID 263`.

```
Mem: 1578756K used, 127032K free, 8744K shrd, 201592K buff, 341348K cached
CPU:   3% usr   3% sys   0% nic  92% idle   0% io   0% irq   0% sirq
Load average: 0.07 0.04 0.08 4/765 45738
  PID  PPID USER     STAT   VSZ %VSZ CPU %CPU COMMAND
    1     0 root     S     1528   0%   0   0% /sbin/init
  235     1 root     S     632m  38%   0   0% PM2 v2.10.3: God Daemon (/root/.pm2)
  263   235 root     S     630m  38%   0   0% node /home/site/wwwroot/app.js
  482   291 root     S     7368   0%   0   0% sshd: root@pts/0
45513   291 root     S     7356   0%   0   0% sshd: root@pts/1
  291     1 root     S     7324   0%   0   0% /usr/sbin/sshd
  490   482 root     S     1540   0%   0   0% -ash
45539 45513 root     S     1540   0%   0   0% -ash
45678 45539 root     R     1536   0%   0   0% top
45733     1 root     Z        0   0%   0   0% [init]
45734     1 root     Z        0   0%   0   0% [init]
45735     1 root     Z        0   0%   0   0% [init]
45736     1 root     Z        0   0%   0   0% [init]
45737     1 root     Z        0   0%   0   0% [init]
45738     1 root     Z        0   0%   0   0% [init]
```

## <a name="next-steps"></a>Volgende stappen

U kunt vragen en problemen boeken op de [Azure-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

Zie voor meer informatie over Web-App voor Containers:

* [Introductie van foutopsporing op afstand van Node.js-apps in Azure App Service vanuit VS-Code](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Een aangepaste Docker-installatiekopie uitvoeren voor Web App for Containers](quickstart-docker-go.md)
* [.NET Core gebruiken in Azure App Service onder Linux](quickstart-dotnetcore.md)
* [Ruby gebruiken in Azure App Service onder Linux](quickstart-ruby.md)
* [Web App for Containers van Azure App Service: veelgestelde vragen](app-service-linux-faq.md)