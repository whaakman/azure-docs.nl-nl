---
title: SSH-ondersteuning voor App Service on Linux - Azure | Microsoft Docs
description: Meer informatie over het gebruik van SSH met Azure App Service on Linux.
keywords: Azure appservice, web-app, linux, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 2d84a4dd0b69ce9ca7fc594dffce3238c620c426
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543970"
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>SSH-ondersteuning voor Azure App Service on Linux

[Secure Shell (SSH)](https://wikipedia.org/wiki/Secure_Shell) wordt meestal gebruikt om op afstand met beheerdersrechten opdrachten uitvoeren vanaf een opdrachtregel terminal. App Service on Linux biedt ondersteuning voor SSH naar de app-container met elk van de ingebouwde Docker-installatiekopieën die worden gebruikt voor de Runtime-Stack van nieuwe web-apps. 

![Runtimestacks](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

Voor aangepaste Docker-installatiekopieën, door het configureren van SSH-server in uw aangepaste installatiekopie.

U kunt ook verbinding maken met de container rechtstreeks vanuit uw lokale ontwikkelcomputer met behulp van SSH- en SFTP.

## <a name="open-ssh-session-in-browser"></a>SSH-sessie openen in browser

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-no-h.md)]

## <a name="use-ssh-support-with-custom-docker-images"></a>SSH-ondersteuning gebruiken met aangepaste Docker-installatiekopieën

Zie [configureren SSH in een aangepaste container](configure-custom-container.md#enable-ssh).

## <a name="open-ssh-session-from-remote-shell"></a>SSH-sessie openen vanuit externe shell

> [!NOTE]
> Deze functie is momenteel in Preview.
>

Met behulp van TCP tunneling u kunt een netwerkverbinding tussen uw ontwikkelcomputer en Web App for Containers via een geverifieerde WebSocket-verbinding maken. Hiermee kunt u een SSH-sessie openen met de container die wordt uitgevoerd in App Service van de client van uw keuze.

Als u wilt beginnen, moet u installeren [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). Als u wilt zien hoe dit werkt zonder de Azure CLI installeren, opent u [Azure Cloud Shell](../../cloud-shell/overview.md). 

Open een externe verbinding met uw app met de [az webapp extern verbinding maken](/cli/azure/ext/webapp/webapp/remote-connection?view=azure-cli-latest#ext-webapp-az-webapp-remote-connection-create) opdracht. Geef  _\<abonnement-id >_,  _\<group-name >_ en \_< app-naam > _ voor uw app.

```azurecli-interactive
az webapp remote-connection create --subscription <subscription-id> --resource-group <resource-group-name> -n <app-name> &
```

> [!TIP]
> `&` aan het einde van de opdracht is alleen voor uw gemak bedoeld; als u Cloud Shell. Deze wordt het proces op de achtergrond uitgevoerd, zodat u de volgende opdracht in de dezelfde shell uitvoeren kunt.

Uitvoer van de opdracht geeft u de informatie die u nodig hebt om een SSH-sessie te openen.

```
Port 21382 is open
SSH is available { username: root, password: Docker! }
Start your favorite client and connect to port 21382
```

Open een SSH-sessie met de container met de client van uw keuze, met behulp van de lokale poort. Het volgende voorbeeld wordt de standaardwaarde [ssh](https://ss64.com/bash/ssh.html) opdracht:

```azurecli-interactive
ssh root@127.0.0.1 -p <port>
```

Wanneer u wordt gevraagd, typt u `yes` om door te gaan met het maken van een verbinding. U wordt vervolgens gevraagd om het wachtwoord. Gebruik `Docker!`, die u eerder is aangegeven.

```
Warning: Permanently added '[127.0.0.1]:21382' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Nadat u bent geverifieerd, ziet u het welkomstscherm van de sessie.

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

U bent nu verbonden met uw connector.  

Probeer die wordt uitgevoerd de [boven](https://ss64.com/bash/top.html) opdracht. U zou het mogelijk om te zien van uw app-proces in de lijst. In de onderstaande voorbeelduitvoer is de categorie met `PID 263`.

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

U kunt vragen en opmerkingen plaatsen op de [Azure-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

Zie voor meer informatie over Web App for Containers:

* [Maak kennis met de foutopsporing op afstand van Node.js-apps in Azure App Service vanuit VS Code](https://medium.com/@auchenberg/introducing-remote-debugging-of-node-js-apps-on-azure-app-service-from-vs-code-in-public-preview-9b8d83a6e1f0)
* [Een aangepaste Docker-installatiekopie uitvoeren voor Web App for Containers](quickstart-docker-go.md)
* [.NET Core gebruiken in Azure App Service onder Linux](quickstart-dotnetcore.md)
* [Ruby gebruiken in Azure App Service onder Linux](quickstart-ruby.md)
* [Web App for Containers van Azure App Service: veelgestelde vragen](app-service-linux-faq.md)
