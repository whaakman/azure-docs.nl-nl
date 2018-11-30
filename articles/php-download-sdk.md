---
title: De Azure SDK voor PHP downloaden
description: Informatie over het downloaden en installeren van de Azure SDK voor PHP.
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: ''
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: 4f2c242e27d8a0ed6687eb4a3510bbce6e07694f
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52421484"
---
# <a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden

## <a name="overview"></a>Overzicht

De Azure SDK voor PHP bevat onderdelen waarmee u kunt ontwikkelen, implementeren en beheren van PHP-toepassingen voor Azure. Met name omvat de Azure SDK voor PHP het volgende:

* **De PHP-clientbibliotheken voor Azure**. Deze klassenbibliotheken bieden een interface voor toegang tot Azure-functies, zoals gegevensbeheerservices en cloudservices.
* **De Azure-opdrachtregelinterface voor Mac, Linux en Windows (Azure CLI)**. Dit is een reeks opdrachten voor het implementeren en beheren van Azure-services, zoals Azure Websites en virtuele Machines van Azure. De Azure CLI-werkzaamheden op elk platform, met inbegrip van de Mac, Linux en Windows.
* **Azure PowerShell (alleen Windows)**. Dit is een set PowerShell-cmdlets voor het implementeren en beheren van Azure-Services, zoals Cloudservices en virtuele Machines.
* **De Azure-Emulators (alleen Windows)**. De reken- en emulators zijn lokale emulators van cloudservices en gegevensbeheerservices waarmee u kunt een toepassing lokaal testen. De Azure-Emulators alleen op Windows worden uitgevoerd.

De volgende secties wordt beschreven hoe u downloaden en installeren van de onderdelen die hierboven worden beschreven.

De instructies in dit onderwerp wordt ervan uitgegaan dat u hebt [PHP] [ install-php] geïnstalleerd.

> [!NOTE]
> U hebt PHP 5.5 of hoger met de PHP-clientbibliotheken voor Azure.
>
>

## <a name="php-client-libraries-for-azure"></a>PHP-clientbibliotheken voor Azure

De PHP-clientbibliotheken voor Azure bieden een interface voor toegang tot Azure-functies, zoals gegevensbeheerservices en cloud services, van een besturingssysteem. Deze bibliotheken kunnen worden geïnstalleerd via de Composer.

Zie voor meer informatie over het gebruik van de PHP-clientbibliotheken voor Azure [over het gebruik van de Blob-Service][blob-service], [over het gebruik van de Tabelservice] [ table-service]en [over het gebruik van de Queue-Service][queue-service].

### <a name="install-via-composer"></a>Via Composer installeren

1. [Installeer Git][install-git]. Op Windows moet u ook de uitvoerbare Git toevoegen aan de omgevingsvariabele PATH.

2. Maak een bestand met de naam **composer.json** in de hoofdmap van uw project en voeg de volgende code toe:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Download **[composer.phar] [ composer-phar]** in de hoofdmap van uw project.

4. Open een opdrachtprompt en voer dit uit in de hoofdmap van uw project

        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell en Azure-Emulators

Azure PowerShell is een set PowerShell-cmdlets voor het implementeren en beheren van Azure-Services (zoals Cloudservices en virtuele Machines). De Azure-Emulators zijn emulators van cloudservices en gegevensbeheerservices waarmee u kunt een toepassing lokaal testen. Deze onderdelen worden ondersteund. alleen Windows.

De aanbevolen manier om Azure PowerShell en de Azure-Emulators te installeren, is met de [Microsoft Web Platform Installer][download-wpi]. Houd er rekening mee dat u kunt ook andere onderdelen van de ontwikkeling, zoals PHP, SQL Server, de Microsoft-Drivers voor SQL Server voor PHP en WebMatrix installeren.

Zie voor meer informatie over het gebruik van Azure PowerShell [over het gebruik van Azure PowerShell][powershell-tools].

## <a name="azure-cli"></a>Azure-CLI

De Azure-CLI is een reeks opdrachten voor het implementeren en beheren van Azure-services, zoals Azure Websites en virtuele Machines van Azure. Zie voor meer informatie over het installeren van Azure CLI [Azure CLI installeren](cli-install-nodejs.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie de [PHP-ontwikkelaarscentrum](https://azure.microsoft.com/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: https://getcomposer.org/composer.phar
[nodejs-org]: https://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: https://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: https://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: https://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: https://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: https://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: https://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: https://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: https://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
