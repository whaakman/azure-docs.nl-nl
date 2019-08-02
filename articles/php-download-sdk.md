---
title: De Azure SDK voor PHP downloaden
description: Meer informatie over het downloaden en installeren van de Azure SDK voor PHP.
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
ms.author: yaya
ms.openlocfilehash: 9dd336ef973b603108aad62c90e4dab385d20317
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "67873885"
---
# <a name="download-the-azure-sdk-for-php"></a>De Azure SDK voor PHP downloaden

## <a name="overview"></a>Overzicht

De Azure SDK voor PHP bevat onderdelen waarmee u PHP-toepassingen voor Azure kunt ontwikkelen, implementeren en beheren. De Azure SDK voor PHP omvat met name het volgende:

* **De PHP-client bibliotheken voor Azure**. Deze klassen bibliotheken bieden een interface voor het openen van Azure-functies, zoals gegevens beheer Services en Cloud Services.
* **De Azure-opdracht regel interface voor Mac, Linux en Windows (Azure CLI)** . Dit is een set opdrachten voor het implementeren en beheren van Azure-Services, zoals Azure websites en Azure Virtual Machines. De Azure CLI werkt op elk platform, waaronder Mac, Linux en Windows.
* **Azure PowerShell (alleen Windows)** . Dit is een set Power shell-cmdlets voor het implementeren en beheren van Azure-Services, zoals Cloud Services en Virtual Machines.
* **De Azure-emulators (alleen Windows)** . De compute-en opslag emulators zijn lokale emulators van Cloud Services en gegevens beheer services waarmee u een toepassing lokaal kunt testen. De Azure-emulators worden alleen uitgevoerd in Windows.

In de volgende secties wordt beschreven hoe u de hierboven beschreven onderdelen downloadt en installeert.

In de instructies in dit onderwerp wordt ervan uitgegaan dat u [php][install-php] hebt geïnstalleerd.

> [!NOTE]
> U moet PHP 5,5 of hoger hebben om de PHP-client bibliotheken voor Azure te kunnen gebruiken.
>
>

## <a name="php-client-libraries-for-azure"></a>PHP-clientbibliotheken voor Azure

De PHP-client bibliotheken voor Azure bieden een interface voor het openen van Azure-functies, zoals gegevens beheer Services en Cloud Services, vanuit elk besturings systeem. Deze bibliotheken kunnen worden geïnstalleerd via de Composer.

Voor informatie over het gebruik van de PHP-client bibliotheken voor Azure raadpleegt u [hoe u de BLOB-service gebruikt][blob-service], [hoe u de Table-service][table-service] gebruikt en [hoe u de Queue-service gebruikt][queue-service].

### <a name="install-via-composer"></a>Installeren via Composer

1. [Installeer Git][install-git]. In Windows moet u ook het bestand Git toevoegen aan de omgevings variabele PATH.

2. Maak een bestand met de naam **Composer. json** in de hoofdmap van het project en voeg de volgende code toe:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Down load **[Composer. Phar][composer-phar]** in de hoofdmap van het project.

4. Open een opdracht prompt en voer deze uit in de project root

        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell-en Azure-emulators

Azure PowerShell is een set Power shell-cmdlets voor het implementeren en beheren van Azure-Services (zoals Cloud Services en Virtual Machines). De Azure-emulators zijn emulators van Cloud Services en gegevens beheer services waarmee u een toepassing lokaal kunt testen. Deze onderdelen worden alleen ondersteund door Windows.

De aanbevolen manier om Azure PowerShell en de Azure-emulators te installeren, is door het [installatie programma voor het micro soft][download-wpi]-webplatform te gebruiken. U kunt er ook voor kiezen om andere ontwikkel onderdelen, zoals PHP, SQL Server, de micro soft-Stuur Programma's voor SQL Server voor PHP en webmatrix te installeren.

Zie [Azure PowerShell gebruiken][powershell-tools]voor meer informatie over het gebruik van Azure PowerShell.

## <a name="azure-cli"></a>Azure-CLI

De Azure CLI is een set opdrachten voor het implementeren en beheren van Azure-Services, zoals Azure websites en Azure Virtual Machines. Zie [de Azure cli installeren](cli-install-nodejs.md)voor meer informatie over het installeren van Azure cli.

## <a name="next-steps"></a>Volgende stappen

Zie het [PHP-ontwikkelaars centrum](https://azure.microsoft.com/develop/php/)voor meer informatie.

[install-php]: https://www.php.net/manual/en/install.php
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
