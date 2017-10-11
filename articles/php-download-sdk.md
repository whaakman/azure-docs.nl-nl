---
title: De Azure SDK voor PHP downloaden
description: Informatie over het downloaden en installeren van de Azure SDK voor PHP.
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: 
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: fd3d28b133ef8e646f5c2f1c1127f654daa61b95
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden
## <a name="overview"></a>Overzicht
De Azure SDK voor PHP omvat onderdelen waarmee u kunt ontwikkelen, implementeren en beheren van PHP-toepassingen voor Azure. De Azure SDK voor PHP bevat met name de volgende:

* **De PHP-clientbibliotheken voor Azure**. Deze klassenbibliotheken biedt een interface voor toegang tot Azure-functies, zoals services voor het beheer van gegevens en cloudservices.  
* **De Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)**. Dit is een reeks opdrachten voor het implementeren en beheren van Azure-services, zoals Azure Websites en Azure Virtual Machines. Het werk van de Azure CLI op elk platform, inclusief Mac, Linux en Windows.
* **Azure PowerShell (alleen Windows)**. Dit is een set PowerShell-cmdlets voor het implementeren en beheren van Azure-Services, zoals Cloudservices en virtuele Machines.
* **De Azure Emulators (alleen Windows)**. De berekenings- en emulators zijn lokale emulators van cloudservices en -gegevens management services waarmee u een toepassing lokaal te testen. De Azure-Emulators alleen op Windows worden uitgevoerd.

De volgende secties wordt beschreven hoe downloaden en installeren van de onderdelen die hierboven worden beschreven.

De instructies in dit onderwerp wordt ervan uitgegaan dat u hebt [PHP] [ install-php] geïnstalleerd.

> [!NOTE]
> U kunt PHP 5.5 of hoger de PHP-clientbibliotheken gebruiken voor Azure moet hebben.
> 
> 

## <a name="php-client-libraries-for-azure"></a>PHP-clientbibliotheken voor Azure
De PHP-clientbibliotheken voor Azure biedt een interface voor toegang tot Azure-functies, zoals services voor het beheer van gegevens en in de cloud services, op een ander besturingssysteem. Deze bibliotheken kunnen worden geïnstalleerd via de Composer.

Zie voor meer informatie over het gebruik van de PHP-clientbibliotheken voor Azure [het gebruik van de Blob-Service][blob-service], [het gebruik van de Tabelservice] [ table-service]en [het gebruik van de Queue-Service][queue-service].

### <a name="install-via-composer"></a>Installeren via de Composer
1. [Installeer Git][install-git].

    > [AZURE.NOTE] In Windows moet u ook de uitvoerbare Git toevoegen aan de omgevingsvariabele PATH.

1. Maak een bestand met de naam **composer.json** in de hoofdmap van uw project en voeg de volgende code toe:
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. Download  **[composer.phar] [ composer-phar]**  in de hoofdmap van uw project.
3. Open een opdrachtprompt en dit in de hoofdmap van uw project uitvoeren
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell en Azure Emulators
Azure PowerShell is een set PowerShell-cmdlets voor het implementeren en beheren van Azure-Services (zoals Cloudservices en virtuele Machines). De Azure-Emulators zijn emulators van cloudservices en -gegevens management services waarmee u een toepassing lokaal te testen. Deze onderdelen worden ondersteund. alleen Windows.

De aanbevolen manier voor het installeren van Azure PowerShell en de Azure-emulator is met de [Microsoft Web Platform Installer][download-wpi]. Houd er rekening mee dat u ook kunt andere onderdelen van de ontwikkeling, zoals PHP, SQL Server, de Microsoft-Drivers voor SQL Server voor PHP en WebMatrix installeren.

Zie voor meer informatie over het gebruik van Azure PowerShell [hoe Azure PowerShell gebruiken][powershell-tools].

## <a name="azure-cli"></a>Azure CLI
De Azure CLI is een reeks opdrachten voor het implementeren en beheren van Azure-services, zoals Azure Websites en Azure Virtual Machines. Zie voor meer informatie over het installeren van Azure CLI [Azure CLI installeren](cli-install-nodejs.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie de [PHP-ontwikkelaarscentrum](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
