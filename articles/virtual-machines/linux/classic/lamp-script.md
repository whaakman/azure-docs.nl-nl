---
title: De extensie CustomScript gebruiken op een virtuele Linux-machine | Microsoft Docs
description: Informatie over het gebruik van de extensie CustomScript voor het implementeren van toepassingen op Linux virtuele Machines in Azure gemaakt met behulp van het klassieke implementatiemodel.
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: cb1fc9a44dc9e57d9cc9f1c546ad937d67e63c2f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Een LAMP-app implementeren met de Azure CustomScript-extensie voor Linux
> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor meer informatie over het implementeren van een licht stack met het Resource Manager-model [hier](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

De Microsoft Azure CustomScript-extensie voor Linux biedt een manier voor het aanpassen van uw virtuele machines (VM's) door het uitvoeren van willekeurige code geschreven in een scripttaal die wordt ondersteund door de virtuele machine (bijvoorbeeld, Python en Bash). Dit biedt een zeer flexibele manier voor het automatiseren van de toepassingsimplementatie op meerdere machines.

U kunt de extensie CustomScript met de Azure portal, Windows PowerShell of de Azure-opdrachtregelinterface (Azure CLI) implementeren.

In dit artikel we gebruiken de Azure CLI voor het implementeren van een eenvoudige toepassing licht voor een VM Ubuntu gemaakt met het klassieke implementatiemodel.

## <a name="prerequisites"></a>Vereisten
In dit voorbeeld maakt u eerst twee virtuele Azure-machines met Ubuntu 14.04 of hoger. De virtuele machines worden genoemd *script vm* en *licht vm*. Unieke namen gebruiken bij het maken van de virtuele machines. Een de CLI-opdrachten uitvoeren wordt gebruikt en een wordt gebruikt om de app licht te implementeren.

U moet ook een Azure Storage-account en een sleutel voor toegang tot deze (u kunt dit opvragen bij de Azure-portal).

Als u informatie over het maken van virtuele Linux-machines in Azure moet verwijzen naar [maken van een virtuele Machine waarop Linux](createportal.md).

De installatie-opdrachten wordt ervan uitgegaan Ubuntu, maar u kunt de installatie voor alle ondersteunde Linux-distro aanpassen.

De script-vm-virtuele machine moet beschikken over de Azure CLI hebt geïnstalleerd, met een verbinding met Azure. Voor hulp bij dit naar verwijzen [installeren en configureren van de Azure-opdrachtregelinterface](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>Uploaden van een script
De extensie CustomScript gebruiken we een script uitvoeren op een externe virtuele machine voor het installeren van de stack licht en maken van een PHP-pagina. Om het script vanaf elke locatie toegang te krijgen moet we het uploaden als een Azure-blob.

### <a name="script-overview"></a>Script-overzicht
Voorbeeld van het script een licht-stack in Ubuntu (inclusief het instellen van een stille installatie van MySQL) geïnstalleerd, schrijft u een eenvoudige PHP-bestand en Apache wordt gestart.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Script uploaden
Sla het script als een tekstbestand, bijvoorbeeld *install_lamp.sh*, en vervolgens te uploaden naar Azure Storage. U kunt dit eenvoudig doen met Azure CLI. Het volgende voorbeeld wordt het bestand naar een storage-container met de naam 'scripts' geüpload. Als de container bestaat niet moet u deze eerst maken.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Maak ook een JSON-bestand dat wordt beschreven hoe u het script downloaden van Azure Storage. Opslaan als *public_config.json* ('mystorage' met de naam van uw opslagaccount vervangen):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>De extensie implementeren
U kunt nu de volgende opdracht gebruiken voor het implementeren van de Linux-extensie CustomScript op de externe virtuele machine met de Azure CLI.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

De vorige opdracht worden gedownload en wordt uitgevoerd de *install_lamp.sh* script op de virtuele machine aangeroepen *licht vm*.

Omdat de app een webserver bevat, moet u een HTTP-luisterpoort op de externe virtuele machine openen met de volgende opdracht.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Bewaking en probleemoplossing
U kunt controleren hoe goed het aangepaste script door te kijken naar het logboekbestand op de externe virtuele machine wordt uitgevoerd. SSH kunt uitvoeren naar *licht vm* en staart van het logboekbestand met de volgende opdracht.

    cd /var/log/azure/customscript
    tail -f handler.log

Nadat u de extensie CustomScript hebt uitgevoerd, kunt u bladeren naar de PHP-pagina die u hebt gemaakt voor meer informatie. De PHP-pagina voor het voorbeeld in dit artikel is *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Aanvullende bronnen
U kunt dezelfde basisstappen om complexere apps te implementeren. In dit voorbeeld is het installatiescript opgeslagen als een openbare blob in Azure Storage. Een veiligere optie zijn voor het opslaan van het script voor installatie als een beveiligde blob met een [Secure Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

Aanvullende bronnen voor Azure CLI-, Linux- en de extensie CustomScript worden naast weergegeven.

[Voor het automatiseren van taken voor het virtuele Linux-machine aanpassen met behulp van de extensie CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux-extensies (GitHub)](https://github.com/Azure/azure-linux-extensions)