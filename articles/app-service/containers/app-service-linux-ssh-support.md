---
title: SSH-ondersteuning voor Azure App Service op Linux | Microsoft Docs
description: Meer informatie over het gebruik van SSH met Azure App Service op Linux.
keywords: Azure app service, web-app, linux, oss
services: app-service
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 66f9988f-8ffa-414a-9137-3a9b15a5573c
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: wesmc
ms.openlocfilehash: 5c877222c9ce409ea8758d5830f79e4a8b64fd8f
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="ssh-support-for-azure-app-service-on-linux"></a>SSH-ondersteuning voor Azure App Service op Linux

[Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) is een cryptografische netwerkprotocol voor het gebruik van netwerkservices veilig. Dit wordt meestal gebruikt voor Meld u aan bij een systeem op afstand veilig vanaf een opdrachtregel en administratieve opdrachten extern uit te voeren.

Op Linux-App Service biedt SSH-ondersteuning in de app-container met elk van de ingebouwde Docker-installatiekopieën gebruikt voor de Runtime-Stack van nieuwe web-apps.

![Runtime-Stacks](./media/app-service-linux-ssh-support/app-service-linux-runtime-stack.png)

U kunt ook SSH gebruiken met aangepaste installatiekopieën Docker door waaronder de SSH-server als onderdeel van de installatiekopie en deze te configureren zoals beschreven in dit onderwerp.

## <a name="making-a-client-connection"></a>Maken van een clientverbinding

Als u een SSH-clientverbinding maken, moet de primaire site worden gestart.

Plak het eindpunt Source Control Management (SCM) voor uw web-app in uw browser de volgende notatie:

```bash
https://<your sitename>.scm.azurewebsites.net/webssh/host
```

Als u zijn niet geverifieerd, moet u met uw Azure-abonnement om verbinding te verifiëren.

![SSH-verbinding](./media/app-service-linux-ssh-support/app-service-linux-ssh-connection.png)

## <a name="ssh-support-with-custom-docker-images"></a>Ondersteuning van SSH met aangepaste Docker-installatiekopieën

Om een aangepaste installatiekopie Docker ter ondersteuning van SSH-communicatie tussen de container en de client in de Azure portal, moet u de volgende stappen uitvoeren voor uw Docker-installatiekopie.

Deze stappen zijn worden weergegeven in de Azure App Service-opslagplaats als [een voorbeeld](https://github.com/Azure-App-Service/node/blob/master/6.9.3/).

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

1. Toevoegen een [ `COPY` instructie](https://docs.docker.com/engine/reference/builder/#copy) naar de Dockerfile kopiëren een [sshd_config](http://man.openbsd.org/sshd_config) van het bestand in de */enzovoort/ssh/* directory. Het configuratiebestand moet worden gebaseerd op onze sshd_config-bestand in de Azure App Service-GitHub-opslagplaats [hier](https://github.com/Azure-App-Service/node/blob/master/8.2.1/sshd_config).

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

1. Zorg ervoor dat u [start de ssh-service](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) met behulp van een shellscript in */bin* directory.

    ```bash
    #!/bin/bash
    service ssh start
    ```

Maakt gebruik van de Dockerfile de [ `CMD` instructie](https://docs.docker.com/engine/reference/builder/#cmd) het script uit te voeren.

    ```docker
    COPY init_container.sh /bin/
    ...
    RUN chmod 755 /bin/init_container.sh
    ...
    CMD ["/bin/init_container.sh"]
    ```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppelingen voor meer informatie over Web-App voor Containers. Vragen kunt u posten op [ons forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Een aangepaste Docker-installatiekopie uitvoeren voor Web App for Containers](quickstart-docker-go.md)
* [.NET Core gebruiken in Azure App Service onder Linux](quickstart-dotnetcore.md)
* [Ruby gebruiken in Azure App Service onder Linux](quickstart-ruby.md)
* [Web App for Containers van Azure App Service: veelgestelde vragen](app-service-linux-faq.md)